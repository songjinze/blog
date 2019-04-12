---
title: 338 Counting Bits
tags: LeetCode
categories: LeetCode
date: 2019-04-04 21:16:48
---


## 题目描述

```
Given a non negative integer number num. For every numbers i in the range 0 ≤ i ≤ num calculate the number of 1's in their binary representation and return them as an array.

Example 1:

Input: 2
Output: [0,1,1]
Example 2:

Input: 5
Output: [0,1,1,2,1,2]
Follow up:

It is very easy to come up with a solution with run time O(n*sizeof(integer)). But can you do it in linear time O(n) /possibly in a single pass?
Space complexity should be O(n).
Can you do it like a boss? Do it without using any builtin function like __builtin_popcount in c++ or in any other language.

```

## 解题思路

用dp的思想

## 源码

```java
class Solution {
    public int[] countBits(int num) {
        int[]res=new int[num+1];
        if(num==0){
            res[0]=0;
            return res;
        }else if(num==1){
            res[0]=0;
            res[1]=1;
            return res;
        }
        res[0]=0;
        res[1]=1;
        int count=4;
        int cur=1;
        int temp=2;
        while(count<=num){
            for(temp=cur+1;temp<count;temp++){
                res[temp]=res[temp&cur]+1;
            }
            cur=count-1;
            count=count<<1;
        }
        for(;temp<=num;temp++){
            res[temp]=res[temp&cur]+1;
        }
        return res;
    }
}
```
