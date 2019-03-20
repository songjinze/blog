---
title: 33 Search in Rotated Sorted Array
date: 2019-03-20 00:35:23
tags: LeetCode
categories: LeetCode
---

## 题目描述

```
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., [0,1,2,4,5,6,7] might become [4,5,6,7,0,1,2]).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of O(log n).

Example 1:

Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
Example 2:

Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

## 解题思路

1. 二分法找到偏移量
2. 根据偏移量将数组分为两个有序数组
3. 根据有序数组分割边界确定在那个数组中进行二分查找

## 源码

```java
class Solution {
    public int search(int[] nums, int target) {
        if(nums.length==0){
            return -1;
        }
        int lo=0,hi=nums.length-1;
        int mid;
        while(lo<hi){
            mid=(lo+hi)/2;
            if(nums[mid]>nums[hi]){
                lo=mid+1;
            }
            else{
                hi=mid;
            }
        }
        if(lo==0){
            return binarySearch(nums,0,nums.length-1,target);
        }
        if(nums[0]>target){
            return binarySearch(nums,lo,nums.length-1,target);
        }else{
            return binarySearch(nums,0,lo-1,target);
        }
    }
    private int binarySearch(int[]nums,int lo,int hi,int target){
        int mid;
        while(lo<=hi){
            mid=(lo+hi)/2;
            if(nums[mid]<target){
                lo=mid+1;
            }else if(nums[mid]>target){
                hi=mid-1;
            }else{
                return mid;
            }
        }
        return -1;
    }
}
```