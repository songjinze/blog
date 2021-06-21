---
title: 11ContainerWithMostWater
date: 2019-02-04 13:08:17
tags: [LeetCode,算法]
categories: [算法]
---


## 题目描述

Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.  

Note: You may not slant the container and n is at least 2.  

{% asset_img 题目描述.PNG 题目描述 %}  

The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.  


Example:  

Input: [1,8,6,2,5,4,8,3,7]  
Output: 49  

## 解题思路

采用solution第二种方法，双指针向中间逼近。

## 源码

```java
class Solution {
    public int maxArea(int[] height) {
        int maxArea=0,left=0,right=height.length-1;
        int width=right-left;
        while(left<right){
            maxArea=Math.max(maxArea,Math.min(height[left],height[right])*(width));
            if(height[left]<=height[right]){
                left++;
            }else{
                right--;
            }
            width--;
        }
        return maxArea;
    }
}
```