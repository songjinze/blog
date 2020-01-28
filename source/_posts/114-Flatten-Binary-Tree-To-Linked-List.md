---
title: 114 Flatten Binary Tree To Linked List
tags: LeetCode
categories: LeetCode
date: 2020-01-20 12:59:09
---


## 题目描述

```
Given a binary tree, flatten it to a linked list in-place.

For example, given the following tree:

    1
   / \
  2   5
 / \   \
3   4   6
The flattened tree should look like:

1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```

## 解题思路

题目中的二叉树实际上通过**前序遍历**即可按照从小到大的顺序排列。
所以解决的问题是如何将其拼接成前序遍历的形式。  
前序遍历的顺序为root -> root.left -> root.right  
因此拼接顺序为 root.right -> root.left -> root ，前者拼接完成后拼接到后者右侧。

## 源码

源码借鉴了讨论区中一种较为清晰而简洁的递归写法。pre表示当前递归完成后的root。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public void flatten(TreeNode root) {
        flatten(root, null);
    }
    private TreeNode flatten(TreeNode root, TreeNode pre){
        if(root == null) return pre;
        pre = flatten(root.right, pre);  // root.right
        pre = flatten(root.left, pre);   // root.right 拼接到 root.left 右侧
        root.right = pre;                // root.left 拼接到 root 右侧
        root.left = null;
        return root;
    }
}
```