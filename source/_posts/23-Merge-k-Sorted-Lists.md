---
title: 23 Merge k Sorted Lists
date: 2019-03-19 23:18:16
tags: [LeetCode,算法]
categories: [算法]
---

## 题目描述

<pre>
Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

Example:

Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
</pre>

## 解题思路

两两合并，复杂度NlogK，K是链表数量。

## 源码

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists.length==0){
            return null;
        }
        int N=lists.length;
        int count=1;
        while(count<N){
            for(int i=0;i<N-count;i=i+count*2){
                lists[i]=mergeTwoLists(lists[i],lists[i+count]);
            }
            count=count<<1;
        }
        return lists[0];
    }
    private ListNode mergeTwoLists(ListNode l1,ListNode l2){
        if(l1==null&&l2==null){
            return null;
        }else if(l1==null){
            return l2;
        }else if(l2==null){
            return l1;
        }
        ListNode res=null;
        ListNode temp=null;
        if(l1.val<l2.val){
            res=l1;
            l1=l1.next;
        }else{
            res=l2;
            l2=l2.next;
        }
        temp=res;
        while(l1!=null&&l2!=null){
            if(l1.val<l2.val){
                temp.next=l1;
                l1=l1.next;
            }else{
                temp.next=l2;
                l2=l2.next;
            }
            temp=temp.next;
        }
        if(l1!=null){
            temp.next=l1;
        }
        if(l2!=null){
            temp.next=l2;
        }
        return res;
    }
}
```