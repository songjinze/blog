---
title: 31 Next Permutation
date: 2019-03-19 23:50:34
tags: [LeetCode,算法]
categories: [算法]
---

## 题目描述

<pre>
Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
</pre>

## 解题思路

<pre>
例如 1 2 3 6 5 4 长度为N
1. 从右至左查找到一个a[i]>a[i-1]，例子中为 3 6 i=i-1
2. 从i向右找到a[j]>a[i]且a[j+1] < a[i]
3. 交换i,j
4. 反转i+1到N-1

</pre>

## 源码

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int N=nums.length;
        int i=N-1;
        for(;i>0;i--){
            if(nums[i]>nums[i-1]){
                break;
            }
        }
        if(i==0){
            reverse(nums,0,N-1);
            return;
        }else{
            i--;
        }
        int j=i+1;
        for(;j<N;j++){
            if(nums[j]<=nums[i]){
                j=j-1;
                break;
            }
        }
        if(j==N){
            j--;
        }
        exch(nums,i,j);
        reverse(nums,i+1,N-1);
    }
    private void reverse(int[]nums,int lo,int hi){
        while(lo<hi){
            exch(nums,lo,hi);
            lo++;
            hi--;
        }
    }
    private void exch(int[]nums,int i,int j){
        int temp=nums[i];
        nums[i]=nums[j];
        nums[j]=temp;
    }
}
```