---
title: Knuth-Morris-Pratt子字符串查找算法
tags: 算法
categories: 算法
date: 2019-05-15 16:10:17
---


## Knuth-Morris-Pratt子字符串查找算法(KMP)

### 来源和基本思想

子字符串查找有一个简单而使用广泛的暴力算法，在最坏情况下在长度为N的文本中查找长度为M的模式需要~NM次字符比较。  
这种暴力算法一般在发生不匹配时可以进行显式回退。

```java
//暴力子字符串查找：pat是字符串匹配的模式，txt是要匹配的字符串
public static int search(String pat, String txt){
    int M = pat.length();
    int N = txt.length();
    for(int i = 0; i <= N - M; i++){
        int j;
        for(j = 0; j < M; j++){
            if(txt.charAt(i + j) != pat.charAt(j))
                break;
        }
        if (j == M) return i; //找到匹配
    }
    return N;                 //未找到匹配
}
```

```java
//暴力子字符串匹配算法的另一种实现（显示回退）
public static int search(String pat, String txt){
    int j, M = pat.length();  //j为模式指针
    int i, N = txt.length();  
    for(i = 0, j = 0; i < N && j < M; i++){
        if(txt.charAt(i) == pat.charAt(j)) j++;
        else {i -= j; j = 0;}
    }
    if(j == M) return i - M;    // 找到匹配
    else       return N;        // 未找到匹配
}
```

### KMP算法思想

与一般暴力方法中的显示回退不同的是，KMP算法基本思想是当出现不匹配时，就能知晓一部分文本的内容。我们可以利用这些信息避免将指针回退到所有这些已经知晓的字符以前。  

举个例子，假设字母表中只有两个字符，需要匹配的字符串为"B A A A A A A A A"，现在假设我们已经匹配了5个字符，第6个字符匹配失败，则指针指向第6个字符为'B'，因为已知除了第一个字符外其他字符都为'A'，实际上我们不需要将指针回退到第一个字符的位置然后加一。  

{% asset_img 回退示例.PNG 回退 %}

### 模式指针回退和DFA模拟

首先我们用一个**针对pat**的二维数组dfa[][]来记录匹配失败时模式指针j（参照上文暴力算法）应该回退多远。  
**对于每个字母表中的字符c，在比较了c和pat.charAt(j)之后，dfa\[c\]\[j\]表示的是下个文本字符比较的模式字符的位置。**  
> 查找时，相当于根据dfa\[txt.charAt(i)\]\[j\]的值找到txt.charAt(i+1)应该在pat中比较的位置（可以理解为将指针移动到的位置），匹配时dfa\[pat.charAt(j)\]\[j\]总是j+1。在不匹配时，已知的dfa相当于已经根据txt之前的j-1个字符定位到了需要回退到的最短距离。

这个过程的难点在于计算dfa数组。在这之前先介绍一种很好的实现：**确定有限状态自动机(DFA)**  

#### DFA模拟

有限状态自动机是由状态（数字标记的圆圈）和转换（带标签的箭头）组成的。  
* 模式中的每个字符都对应着一个状态。
* 每个状态的转换只有一条匹配转换（从j到j+1），其他都是非匹配转换（指向之前的左侧状态），非匹配转换意味着回退。

{% asset_img DFA.PNG DFA %}

```java
//DFA模拟的KMP子字符串查找算法
public int search(String txt){
    int i, j, N = txt.length();
    for(i = 0, j = 0; i < N && j < M; i++){
        // DFA内部的转换过程
        j = dfa[txt.charAt(i)][j];
    }
    // 转换完成，如果当前状态为DFA的结束状态则匹配成功，否则匹配失败
    if(j == M) return i - M; //找到匹配
    else       return N;     //未找到匹配
}
```

#### DFA的构造

接下来就是KMP的核心问题：如何构造DFA

```java
dfa[pat.charAt(0)][0] = 1;
for(int X = 0, j = 1; j < M; j++){
    // 计算dfa[][j]
    for(int c = 0; c < R; c++){
        dfa[c][j] = dfa[c][X];
    }
    dfa[pat.charAt(j)][j] = j + 1;
    X = dfa[pat.charAt(j)][X];
}
```

对于每一个j：
* 将dfa[][X]复制到dfa[][j]，(对于匹配失败的情况)
* 将dfa\[pat.charAt(j)\]\[j\]设为j+1(对于匹配成功的情况)
* 更新X。

## 完整KMP算法

```java
public class KMP{
    private String pat;
    private int[][] dfa;
    public KMP(String pat){
        //由模式字符串构造DFA
        this.pat = pat;
        int M = pat.length();
        int R = 256;
        dfa = new int[R][M];
        dfa[pat.charAt(0)][0] = 1;
        for(int X = 0, j = 1; j < M; j++){
            //计算dfa[][j]
            for(int c = 0; c < R; c++){
                dfa[c][j] = dfa[c][X];      //复制匹配失败情况下的值
            }
            dfa[pat.charAt(j)][j] = j + 1;  //设置匹配成功情况下的值
            X = dfa[pat.charAt(j)][X];      //更新重启状态
        }
    }
    public int search(String txt){
        //在txt上模拟DFA的运行
        int i, j, N = txt.length(), M = pat.length();
        for(i = 0, j = 0;i < N && j < M; i++){
            j = dfa[txt.charAt(i)][j];
        }
        if(j == M) return i - M;    //找到匹配（到达模式字符串的结尾）
        else       return N;        //未找到匹配
    }
}
```

## 性能

对于长度为M的模式字符串和长度为N的文本，KMP字符串查找访问的字符不会超过M+N个。