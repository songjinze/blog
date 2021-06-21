---
title: 771Jewels and Stones
tags: [LeetCode,算法]
categories: [算法]
date: 2019-02-03 01:07:06
---


## 题目描述

You're given strings J representing the types of stones that are jewels, and S representing the stones you have.  Each character in S is a type of stone you have.  You want to know how many of the stones you have are also jewels.
The letters in J are guaranteed distinct, and all characters in J and S are letters. Letters are case sensitive, so "a" is considered a different type of stone from "A".  
Example 1:  
Input: J = "aA", S = "aAAbbbb"  
Output: 3  
Example 2:  
Input: J = "z", S = "ZZ"  
Output: 0  
Note:  
S and J will consist of letters and have length at most 50.  
The characters in J are distinct.  

## 解题思路

理用哈希法可以大幅加快查找速度。

## 源码

```java
class Solution {
    public int numJewelsInStones(String J, String S) {
        if(J.length()==0||S.length()==0||J==null||S==null){
            return 0;
        }
        int[]hash=new int[256];
        for(char c:J.toCharArray()){
            hash[c]++;
        }
        int res=0;
        for(char s:S.toCharArray()){
            if(hash[s]>0){
                res++;
            }
        }
        return res;
    }
}
```