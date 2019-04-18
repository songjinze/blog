---
title: 215 Kth Largest Element in an Array
date: 2019-04-18 18:46:53
tags: LeetCode
categories: LeetCode
---



## 题目描述

```
Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.
Example 1:
Input: [3,2,1,5,6,4] and k = 2
Output: 5
Example 2:
Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
Note: 
You may assume k is always valid, 1 ≤ k ≤ array's length.
```

## 解题思路

建立一个K大小的堆。
> 注释掉的堆排序方法速度较慢，实际上每次插入一个数只使用下沉（sink）操作即可。二者作用相同。

## 源码

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        int[] heap=new int[k+1];
        for(int i=1;i<=k;i++){
            heap[i]=Integer.MIN_VALUE;
        }
        for(int n:nums){
            if(n<heap[1]){
                continue;
            }else{
                heap[1]=n;
                simpleSink(heap,1,k);
                //heapSort(heap);
            }
        }
        return heap[1];
    }
    
    private void simpleSink(int[]nums,int k,int N){
        int j;
        while(k*2<=N){
            j=k*2;
            if(j<N&&less(nums,j+1,j)) j++;
            if(less(nums,k,j)) break;
            exch(nums,k,j);
            k=j;
        }
    }
    private boolean less(int[]nums,int i,int j){
        return nums[i]<nums[j];
    }
    private void exch(int[]nums,int i,int j){
        int temp=nums[i];
        nums[i]=nums[j];
        nums[j]=temp;
    }
    // private void heapSort(int[]nums){
    //     int N=nums.length-1;
    //     for(int k=N/2;k>=1;k--){
    //         sink(nums,k,N);
    //     }
    //     while(N>1){
    //         exch(nums,1,N--);
    //         sink(nums,1,N);
    //     }
    // }
    // private void sink(int[]nums,int k,int N){
    //     int j;
    //     while(k*2<=N){
    //         j=k*2;
    //         if(j<N&&less(nums,j,j+1)) j++;
    //         if(!less(nums,k,j)) break;
    //         exch(nums,k,j);
    //         k=j;
    //     }
    // }
    
}
```