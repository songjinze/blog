---
title: Rabin-Karp子字符串查找算法
tags: 算法
categories: 算法
date: 2019-06-13 02:01:34
---


## Rabin-Karp子字符串查找算法

M.O.Rabin和R.A.Karp发明了一种完全不同的基于散列的字符串查找算法。

### 基本思想

我们假设长度为M的字符串对应着一个R进制的M位数（虽然一个直观想法是每个字符对应一位，但从基本思想来说还有其他可能的计算方法）。  
为了用一张大小为Q的散列表来保存这种类型的键，需要一个能够将R进制的M位数转化为一个0到Q-1之间的int值散列函数。这里可以用除留余数法（将该数除以Q并取余）在实际应用中会使用一个随机的素数Q，在不溢出的情况下选择一个尽可能大的值。  
  
例如：  
在文本3 1 4 1 5 9 2 6 5 4 5 8 9 7 9 3中找到模式2 6 5 3 5，首先选择散列表大小为Q（这个例子中为997），散列值为26535 % 997 = 613，然后计算文本中所有长度为5个数字的子字符串的散列值并寻找匹配。  

{% asset_img 算法查找基本思想.PNG 算法查找基本思想 %}

### 计算散列函数

一个通用的方法是Horner方法：
```java
private long hash(String key, int M){
    //计算key[0..M-1]的散列值
    long h = 0;
    for(int j = 0; j < M; j++){
        h = (R * h + key.charAt(j)) % Q;
    }
    return h;
}
```

{% asset_img Horner方法.PNG Horner方法计算散列值 %}

> 取余操作的一个基本性质是如果在每次算术操作之后都将结果除以Q并取余，这等价于在完成了所有算术操作之后再将最后的结果除以Q并取余。Horner实现除留余数法时利用了这个性质。这么做的结果是无论M是5、100、1000（M指上文M位数），都可以在常数时间内高效地不断向右一格一格地移动。

### 关键思想

我们用t<sub>i</sub>表示txt.charAt(i),那么文本txt中起始于位置i的含有M个字符的子字符串所对应的数即为：  
x<sub>i</sub> = t<sub>i</sub>R<sup>M-1</sup>+t<sub>i+1</sub>R<sup>M-2</sup>+...+t<sub>i+M-1</sub>R<sup>0</sup>  
假设已知h(x<sub>i</sub>)=x<sub>i</sub> mod Q  
则模式字符串右移一位可以替换为：  
x<sub>i+1</sub> = (x<sub>i</sub>-t<sub>i</sub>R<sub>M-1</sub>)R+t<sub>i+M</sub>  
即将它减去第一个数字的值，乘以R，再奸商最后一个数字的值。

### 用蒙特卡洛法验证正确性

使用散列函数方法的问题是可能存在散列值相同而实际两个字符串不匹配的问题（简称**冲突**）。而如果进行匹配检验又比较耗费性能。  
作为替代，这里将散列表地Q设为任意大的一个值，因为这里不会真的构造一张散列表而是希望用模式字符串验证是否会产生上述**冲突**问题。这里会取大于10<sup>20</sup>的long型值，使得一个随机键的散列值与模式字符串冲突的概率小于10<sup>-20</sup>，如果嫌这个值不够小可以运行这个方法两遍，冲突概率小于10<sup>-40</sup>。  
这是*蒙特卡洛*算法一种著名的早期应用，既保证运行时间又使失败概率非常的小。  
检查匹配的其他方法可能很慢（性能有很小的概率相当于暴力算法），这种算法被称为*拉斯维加斯*算法。

### 算法实现

```java
public class RabinKarp{
    private String pat;     //模式字符串（仅拉斯维加斯算法需要）
    private long patHash;   //模式字符串的散列值
    private int M;          //模式字符串的长度
    private long Q;         //一个很大的素数
    private int R = 256;    //字母表的大小
    private long RM;        //R^(M-1) % Q

    public RabinKarp(String pat){
        this.pat = pat;
        this.M = pat.length();
        Q = longRandomPrime();
        RM = 1;
        for(int i= 1; i <= M - 1; i++){
            RM = (R * RM) % Q;
        }
        patHash = hash(pat, M);
    }
    public boolean check(int i){             //蒙特卡洛算法
        return true;                         //拉斯维加斯算法检查是否匹配（这里没实现）
    } 
    private long hash(String key, int M)     //见上文
    private int search(String txt){
        //在文本中查找相等的散列值
        int N = txt.length();
        long txtHash = hash(txt, M);
        if(patHash == txtHash && check(0)) return 0;//一开始就匹配成功
        for(int i = M; i < N; i++){
            //减去第一个数字，加上最后一个数字，再次检查匹配
            txtHash = (txtHash + Q - RM * txt.charAt(i - M) % Q) % Q;
            txtHash = (txtHash * R + txt.charAt(i)) % Q;
            if(patHash == txtHash){
                if(check(i - M + 1))
                    return i - M + 1;               //找到匹配
            }
        }
        return N;
    }
}
```

### 性能分析

1. 使用蒙特卡洛算法的Rabin-Karp子字符串查找算法的运行时间是线性级别的且出错概率极小。
2. 使用拉斯维加斯算法的Rabin-karp子字符串查找算法能够保证正确性且性能极其接近线性级别。

### 子字符串查找算法总结

|算法|版本|最坏情况|一般情况|在文本中回退|正确性|额外的空间需求|
|---|---|---|---|---|---|---|
|暴力算法||MN|1.1N|是|是|1|
|Knuth-Morris-Pratt算法|完整的DFA|2N|1.1N|否|是|MR|
|Knuth-Morris-Pratt算法|仅构造不匹配的状态转换|3N|1.1N|否|是|M|
|Knuth-Morris-Pratt算法|完整版本|3N|N/M|是|是|R|
|Boyer-Morre算法|启发式的查找不匹配的字符|M/N|N/M|是|是|R|
|Rabin-Karp算法|蒙特卡洛算法|7N|7N|否|是|1|
|Rabin-Karp算法|拉斯维加斯算法|7N|7N|是|是|1|