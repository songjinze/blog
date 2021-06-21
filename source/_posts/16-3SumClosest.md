---
title: 16 3SumClosest
tags: [LeetCode,算法]
categories: [算法]
date: 2019-02-08 21:06:39
---


## 题目描述

Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.  

Example:  

Given array nums = [-1, 2, 1, -4], and target = 1.  

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).  

## 解题思路

参照3Sum。

## 源码

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int dis=Integer.MAX_VALUE;
        int closest=0;
        int length=nums.length;
        int lo=0,hi=0;
        int sum=0;
        for(int i=0;i<length-2;i++){
            if(i==0||nums[i]!=nums[i-1]){
                lo=i+1;
                hi=length-1;
                while(lo<hi){
                    sum=nums[i]+nums[lo]+nums[hi];
                    if(sum<target){
                        lo++;
                    }
                    else if(sum>target){
                        hi--;
                    }else{
                        return target;
                    }
                    int newDis=Math.abs(sum-target);
                    if(newDis<dis){
                        closest=sum;
                        dis=newDis;
                    }
                }
            }
        }
        return closest;
    }
}
```