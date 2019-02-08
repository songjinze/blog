---
title: 198 HouseRobber
tags: LeetCode
categories: LeetCode
date: 2019-02-09 01:13:30
---


## 题目描述

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.  

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.  

Example 1:  

Input: [1,2,3,1]  
Output: 4  
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).  
             Total amount you can rob = 1 + 3 = 4.  
Example 2:  

Input: [2,7,9,3,1]  
Output: 12  
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).  
             Total amount you can rob = 2 + 9 + 1 = 12.  

## 解题思路

此题是一个很好的练手题。很考验对于dp等问题的多种解法的理解。
这里采用一种dp的常规思路。

## 源码

```java
class Solution {
    public int rob(int[] nums) {
        int N=nums.length;
        if(N==0){
            return 0;
        }else if(N==1){
            return nums[0];
        }
        int[] dp=new int[N+1];
        dp[0]=0;
        dp[1]=nums[0];
        for(int i=2;i<=N;i++){
            int val=nums[i-1];
            dp[i]=Math.max(val+dp[i-2],dp[i-1]);
        }
        return dp[N];
    }
}
```

