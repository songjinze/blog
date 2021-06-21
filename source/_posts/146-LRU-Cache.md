---
title: 146 LRU Cache
tags: [LeetCode,算法]
categories: [算法]
date: 2019-03-13 03:33:53
---


## 题目描述

<pre>
Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: get and put.

get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
put(key, value) - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

Follow up:
Could you do both operations in O(1) time complexity?

Example:

LRUCache cache = new LRUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
</pre>

## 解决思路

使用双向链表+哈希表。这是一道典型的LRU问题（Least Recently Used），意思就是最近最少使用。采用这种方式淘汰掉key。

## 源码

```java
import java.util.Hashtable;
class LRUCache {

    class Node{
        int key;
        int value;
        Node next=null;
        Node pre=null;
        Node(int key,int value){
            this.key=key;
            this.value=value;
        }
    }
    
    private void removeNode(Node node){
        node.next.pre=node.pre;
        node.pre.next=node.next;
    }
    
    private void addNode(Node node){
        head.next.pre=node;
        node.next=head.next;
        head.next=node;
        node.pre=head;
    }
    
    private void moveToHead(Node node){
        removeNode(node);
        addNode(node);
    }
    
    private Node head,tail;
    private Hashtable<Integer,Node>cache;
    private int count;
    private int capacity;
    public LRUCache(int capacity) {
        cache=new Hashtable<>(capacity);
        head=new Node(-1,-1);
        tail=new Node(-1,-1);
        head.next=tail;
        tail.pre=head;
        this.capacity=capacity;
        count=0;
    }
    
    public int get(int key) {
        Node node=cache.get(key);
        if(node==null){
            return -1;
        }else{
            moveToHead(node);
            return node.value;
        }
    }
    
    
    public void put(int key, int value) {
        Node node=cache.get(key);
        if(node!=null){
            node.value=value;
            moveToHead(node);
            return;
        }else{
            node=new Node(key,value);
            addNode(node);
            if(count==capacity){
                cache.remove(tail.pre.key);
                removeNode(tail.pre);
            }else{
                count++;
            }
            cache.put(key,node);
        }
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```