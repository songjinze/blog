---
title: 使用伪对象(Mock Object)实现接口测试
tags: 个人经验
categories: 软件开发
date: 2018-12-26 19:58:53
---


# 使用Mock Object进行TDD

在编写单元测试的时候很容易遇到这样的问题：
* 对于接口的调用涉及到多层调用
* 接口的实现还没完成
* 涉及到对于硬件、数据库的操作，使用大数据量的测试很占用机器资源，测试运行较复杂  

这样的问题如果你不以TDD的方式进行编程的话，可能不是一个显著的问题（比如你先把底层实现了之后再写上层），而在TDD中是一定会发生的。这时候就必须引入Mock Object（伪对象）来模拟接口的实现。

## 什么是TDD？

TDD（Test Driven Development），即**测试驱动开发**。它是敏捷开发的最重要的部分。方法主要是先依据客户的需求编写测试程序，然后再编码使其通过测试。在敏捷开发实施中，开发人员主要从两个方面去理解测试驱动开发：
* 在测试的辅助下，快速实现客户需求的功能。通过编写测试用例，对客户需求的功能进行分解，并进行系统设计。从使用角度对代码的设计同城更符合后期开发的需求。可测试的要求，对代码的内聚性的提高和复用都非常有益。
* 在测试的保护下，不断重构代码，提高代码的重用性，从而提高软件产品的质量。可见测试驱动开发实施的好坏确实极大的影响软件产品的质量，贯穿了软件开发的始终。  

在实际开发中，对于接口的测试（而不是对于具体类的测试）是TDD的主要方式，因此接口的稳定性非常重要。接口的变化带来的变化是巨大的。在设计阶段就应该保证接口的稳定性。

## 使用Mock Object进行接口测试

如上文所说，在需要和系统内的某个模块或实体进行交互的时候，这些模块和实体可能并不存在，这时候开发人员需要使用MO技术进行单元测试。  

例如：我们有一个电视机的接口程序：（源码参考IBM Developer）  

```java
public interface VideoCardInterface{
    public void open();
    public void changeChannel(int i);
    public void close();
    public Byte[] read();
    public boolean fault();
}
```

在编写MO的时候，初学者很容易犯的一个错误是对于MO的轻视所造成的编写的不完整。由于MO是为了测试服务的，在开发人员手动编写MO时，应该保证MO实现原接口的所有逻辑功能。由于测试是以接口方法为单位，并且包括方法间上下文调用的，因此可能会存在上下文的逻辑错误。MO的编写应该能使测试发现这样的问题。

```java
public class MockVCHandler implements VideoCardInterface { 
 
    private boolean initialized = false; 
    private boolean error = false; 
    private int channel; 
    private static final int DEFAULTCHANNEL = 1; 
 
    public void open() { 
        initialized = true; 
        channel = DEFAULTCHANNEL; 
    } 
 
    public void changeChannel(int i) { 
        if (!initialized) { 
            Assert.fail("Trying to change channel before open"); 
        } 
        if (i <= 0) { 
            Assert.fail("Specified channale is out-of-range"); 
        } 
        this.channel = i; 
    } 
 
    public void close() { 
        if (!initialized) { 
            Assert.fail("Trying to close before open"); 
        } 
    } 
 
    public Byte[] read() { 
        if (!initialized) { 
            Assert.fail("Trying to read before open"); 
            return null; 
        } 
        if (channel > 256) { 
            error = true; 
            Assert.fail("Channel is out-of-range"); 
        } 
        return new Byte[] { '0', '1' }; 
    } 
 
    public boolean fault() { 
        return error; 
    } 
}

```

由此可见，手动编写MO并不是一件容易的事情。实际上我们发现MO可编写应该根据MO的实际作用得到简化。MO的作用无非是：
* 模拟接口方法的返回值
* 模拟接口方法调用后可能会有的影响  

这意味着，接口方法内部实现不应是MO关心的地方。因此，一些MO的框架应运而生。目前Java的主要Mock Object测试工具有：jMock、MockCreator、MockRunner、EasyMock、MockMaker等。在微软.Net阵营中主要是：NMock、.NetMock、Rhino Mocks和Moq等。  

### jMock框架的使用

jMock是一个轻量级的模拟对象技术的实现。它的特点包括：
* 可以用简单易行的方法定义模拟对象，无需破坏本来的代码结构表。  
* 可以定义对象之间的交互，从而增强测试的稳定性。
* 可以集成到测试框架。
* 易扩充。  

jMock可以添加maven依赖或者在官网中下载。  

#### 使用jMock接口

使用框架的好处是，框架使用对象来模拟上下文，上下文可以模拟出对象和对象的输出，并且还可以检测应用是否合法。

需要进行模拟的接口：  

```java
public interface ICalculatorService { 
 
    public int add(int a, int b); 
 
}
```

使用jMock可以对接口方法返回值进行模拟：  

```java
import org.jmock.Mockery; 
import org.jmock.Expectations; 
 
public class AJmockTestCase { 
 
    Mockery context = new Mockery(); 
 
    public void testCalcService() { 
 
        // set up 
        final ICalculatorService calcService = context 
                .mock(ICalculatorService.class); 
 
        final int assumedResult = 2; 
 
        // expectations 
        context.checking(new Expectations() { 
            { 
                oneOf(calcService).add(1, 1); 
                will(returnValue(assumedResult)); 
            } 
        }); 
 
        System.out.println(calcService.add(1, 1)); 
 
    } 
 
}
```