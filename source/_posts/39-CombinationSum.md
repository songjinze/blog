---
title: 39 CombinationSum
tags: LeetCode
categories: LeetCode
date: 2019-04-18 17:43:46
---


## 题目描述

```
Given a set of candidate numbers (candidates) (without duplicates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.
The same repeated number may be chosen from candidates unlimited number of times.
Note:
All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.
Example 1:
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
Example 2:
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

## 解题思路

回溯法

## 源码

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> res=new ArrayList<>();
        backTrace(res,new ArrayList<>(),0,candidates,target);
        return res;
    }
    private void backTrace(List<List<Integer>>list,List<Integer>temp,int index,int[]candidates,int target){
        if(target==0){
            list.add(new ArrayList(temp));
            return;
        }else if(target<0){
            return;
        }else if(index==candidates.length){
            return;
        }
        for(int i=index;i<candidates.length;i++){
            temp.add(candidates[i]);
            backTrace(list,temp,i,candidates,target-candidates[i]);
            temp.remove(temp.size()-1);
        }
    }
}
```