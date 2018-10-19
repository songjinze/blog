---
title: Shell编程
date: 2018-10-17 14:35:09
tags: 课程学习
categories: Linux
---

# Shell编程

通常情况下，从命令行每输入一次命令就能够得到系统响应，如果需要一个接一个地输入命令才得到结果的时候，这样的做法效率很低。使用Shell程序或者Shell脚本可以很好地解决这个问题。  

## 熟悉Shell程序的创建

作为命令语言互动式地解释和执行用户输入的命令是Shell的功能之一，Shell还可以用来进行程序设计，它提供了定义变量和参数的手段以及丰富的过程控制结构。使用Shell编程类似于使用DOS中的批处理文件，称为Shell脚本，又叫做Shell程序或Shell命令文件。  

### 语法基本介绍

Shell程序基本语法较为简单，主要由开头部分、注释部分以及语句执行部分组成。  

#### 开头

Shell程序必须以下面的行开始（必须放在文件的第一行）。  
\#!/bin/bash  
符号“#!”用来告诉系统它后面的参数是用来执行该文件的程序，在这个例子中使用/bin/bash来执行程序。当编辑好脚本时，如果要执行该脚本，还必须使其可执行。  
要使脚本可执行，需赋予该文件可执行的权限，使用：  
chmod u+x [文件名]  

#### 注释

在进行Shell编程时，以“#”开头的句子表示注释，直到这一行的结束，建议在程序中使用注释。如果使用注释，那么即使相当长的时间内没有使用该脚本，也能在很短的时间内明白该脚本的作用及工作原理。  

### 一个简单的Shell程序的创建过程

Shell程序就是放在一个文件中的一系列Linux命令和实用程序，在执行的时候，通过Linux系统一个接一个地解释和执行每个命令，这和Windows系统下地批处理程序非常相似。

#### 创建文件

在/root目录下使用vi编辑器创建文件date，该文件内容如下所示，共有三个命令。  

\#!/bin/bash  
\#filename:date  
echo "Mr.$USER,Today is:"  
date  
echo Whish you a lucky day!  

#### 设置可执行权限

创建完date文件之后它还不能执行，需要给它设置可执行权限，使用如下命令可给文件设置权限。  
chmod u+x /root/date  
ls -l /root/date  

#### 执行Shell程序

输入整个文件的完整路径执行Shell程序，使用如下命令执行。  
/root/date  

#### 使用bash命令执行程序

在执行Shell程序时需要将date文件设置为可执行权限。如果不设置文件的可执行权限，那么需要使用bash命令告诉系统它是一个可执行的脚本，使用如下命令执行Shell程序。  

### 显示欢迎界面的Shell程序

通过上一节的实例可以掌握整个Shell程序编写和执行的方法。  

在/root目录下使用vi编辑器创建文件welcome，在该文件中输入以下内容。  

\#!/bin/bash  
\#filename:welcome  
first()  
{  
echo "=========="  
echo "Hello Everyone! Welcome to the Linux world."  
echo "=========="  
}  
second()  
{  
echo "************"  
}  
first  
second  
second  
first  

创建完后使用bash /root/welcome执行Shell程序。  

## Shell变量

像高级程序设计语言一样，Shell也提供说明和使用变量的功能。对Shell来讲，所有变量的取值都是一个字符，Shell程序采用“$var”的形式来引用名为var的变量的值。  

### Shell定义的环境变量

Shell在开始执行时就已经定义了一些与系统的工作环境有关的变量，用户还可以重新定义这些变量，常用的Shell环境变量如下：  
HOME：用户保存用户宿主目录的完全路径名。  
PATH：默认命令搜索路径。  
TERM：终端的类型。  
UID：当前用户的识别号。  
PWD：当前工作目录的绝对路径名。  
PS1：用户平时的提示符。  
PS2：第一行没输完，等待第二行输入的提示符。  
