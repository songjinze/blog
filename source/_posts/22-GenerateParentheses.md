---
title: 22 GenerateParentheses
tags: [LeetCode,算法]
categories: [算法]
date: 2019-02-09 00:51:43
---


## 题目描述

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.  

For example, given n = 3, a solution set is:  

[  
  "((()))",  
  "(()())",  
  "(())()",  
  "()(())",  
  "()()()"  
]  

## 解题思路

采用回溯算法的思想，实现上很像DFS。  
此方法超过100%，用时1ms。在优化上主要两点：
* 采用全局变量（结果list和中途使用的保存临时变量的数组，递归过程只使用引用和数组位置的索引。）  
* 由于类似于DFS，因此可以在递归过程中只使用一个数组保存数据。（正确性请自行验证）

## 源码

```java
class Solution {
    private List<String>res=new ArrayList<>();
    
    public List<String> generateParenthesis(int n) {
        if(n==0){
            res.add("");
        }else{
            char[] chars;chars=new char[n*2];
            generate(chars,0,n,n);
        }
        return res;
    }
    private void generate(char[] chars,int index,int l,int r){
        if(r==0){
            res.add(new String(chars));
        }
        if(l>0){
            chars[index]='(';
            generate(chars,index+1,l-1,r);
        }
        if(r>l){
            chars[index]=')';
            generate(chars,index+1,l,r-1);
        }
    }
}
```