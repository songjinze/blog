---
title: 62 Unique Paths
tags: LeetCode
categories: LeetCode
date: 2019-04-18 18:02:57
---


## 题目描述

```
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).
The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).
How many possible unique paths are there?
```

## 解题思路

采用动态规划

```
Runtime: 0 ms, faster than 100.00% of Java online submissions for Unique Paths.
Memory Usage: 31.9 MB, less than 100.00% of Java online submissions for Unique Paths.
```

## 源码

```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp=new int[m][n];
        int i,j=0;
        for(i=0;i<n;i++){
            dp[0][i]=1;
        }
        for(i=0;i<m;i++){
            dp[i][0]=1;
        }
        for(i=1;i<m;i++){
            for(j=1;j<n;j++){
                dp[i][j]=dp[i-1][j]+dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
}
```