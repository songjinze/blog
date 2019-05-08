---
title: 148 Sort List
tags: LeetCode
categories: LeetCode
date: 2019-03-19 01:46:00
---


## 题目描述

<pre>
Sort a linked list in O(n log n) time using constant space complexity.

Example 1:

Input: 4->2->1->3
Output: 1->2->3->4
Example 2:

Input: -1->5->3->4->0
Output: -1->0->3->4->5
</pre>

## 解题思路

利用二分归并排序

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
    public ListNode sortList(ListNode head) {
        if(head==null||head.next==null){
            return head;
        }
        ListNode pre=null,slow=head,fast=head;
        while(fast!=null&&fast.next!=null){
            pre=slow;
            slow=slow.next;
            fast=fast.next.next;
        }
        pre.next=null;
        ListNode l1=sortList(head);
        ListNode l2=sortList(slow);
        return merge(l1,l2);
    }
    private ListNode merge(ListNode l1,ListNode l2){
        if(l1==null&&l2==null){
            return null;
        }
        if(l1==null){
            return l2;
        }else if(l2==null){
            return l1;
        }
        ListNode res,cur;
        if(l1.val<l2.val){
            res=l1;
            l1=l1.next;
        }else{
            res=l2;
            l2=l2.next;
        }
        cur=res;
        while(l1!=null&&l2!=null){
            if(l1.val<l2.val){
                cur.next=l1;
                l1=l1.next;
            }else{
                cur.next=l2;
                l2=l2.next;
            }
            cur=cur.next;
        }
        while(l1!=null){
            cur.next=l1;
            l1=l1.next;
            cur=cur.next;
        }
        while(l2!=null){
            cur.next=l2;
            l2=l2.next;
            cur=cur.next;
        }
        return res;
    }
}
```