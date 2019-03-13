---
title: 494 Target Sum
date: 2019-03-13 19:36:36
tags: LeetCode
categories: LeetCode
---

## 题目描述

<pre>
You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols + and -. For each integer, you should choose one from + and - as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target S.

Example 1:
Input: nums is [1, 1, 1, 1, 1], S is 3. 
Output: 5
Explanation: 

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.
Note:
The length of the given array is positive and will not exceed 20.
The sum of elements in the given array will not exceed 1000.
Your output answer is guaranteed to be fitted in a 32-bit integer.
</pre>

## 解题思路

用动态规划

## 源码

```java
class Solution {
    public int findTargetSumWays(int[] nums, int S) {
        int sum=0;
        for(int n:nums){
            sum+=n;
        }
        
        if(sum<Math.abs(S)){
            return 0;
        }
        
        int i=nums.length;
        int j=sum*2+1;
        int [][]dp=new int[i+1][j];
        int count=1;
        dp[0][sum]=1;
        for(int n:nums){
            for(int k=n;k<j;k++){
                dp[count][k]=dp[count-1][k-n];
            }
            for(int k=0;k<j-n;k++){
                dp[count][k]+=dp[count-1][k+n];
            }
            count++;
        }
        return dp[i][sum-Math.abs(S)];
    }
}

```