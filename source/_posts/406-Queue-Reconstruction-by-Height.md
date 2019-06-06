---
title: 406 Queue Reconstruction by Height
date: 2019-05-15 16:06:55
tags: LeetCode
categories: LeetCode
---

## 题目描述

```
Suppose you have a random list of people standing in a queue. Each person is described by a pair of integers (h, k), where h is the height of the person and k is the number of people in front of this person who have a height greater than or equal to h. Write an algorithm to reconstruct the queue.

Note:
The number of people is less than 1,100.


Example

Input:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

Output:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
```

## 解题思路

1. 现针对h进行从高到低排序，相同h情况下按照k从低到高排序
2. 按排好序的数组，依次将每个元素插入到最终结果的k位置

```
Details 
Runtime: 5 ms, faster than 99.54% of Java online submissions for Queue Reconstruction by Height.
Memory Usage: 38.5 MB, less than 98.40% of Java online submissions for Queue Reconstruction by Height.
```

## 源码

```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        sortByHeight(people,0,people.length-1);
        int[][]res=new int[people.length][2];
        LinkedList<Node> list=new LinkedList<>();
        for(int[] i:people){
            list.add(i[1],new Node(i[0],i[1]));
        }
        int count=0;
        for(Node n:list){
            res[count++]=new int[]{n.h,n.k};
        }
        return res;
    }
    private class Node{
        int h;
        int k;
        Node(int h,int k){
            this.h=h;
            this.k=k;
        }
    }
    private void sortByHeight(int[][]people,int lo,int hi){
        if(lo>=hi){
            return;
        }
        int p=participate(people,lo,hi);
        sortByHeight(people,lo,p-1);
        sortByHeight(people,p+1,hi);
    }
    private int participate(int[][]people,int lo,int hi){
        int[] temp=people[lo];
        int i=lo+1;
        int j=hi;
        while(true){
            while(people[i][0]>temp[0]||(people[i][0]==temp[0]&&people[i][1]<temp[1])) if(i==hi) break;else i++;
            while(people[j][0]<temp[0]||(people[j][0]==temp[0]&&people[j][1]>temp[1])) if(j==lo) break;else j--;
            if(i>=j) break; 
            exch(people,i,j);
        }
        exch(people,j,lo);
        return j;
    }
    private void exch(int[][]people,int i,int j){
        int[]temp=people[i];
        people[i]=people[j];
        people[j]=temp;
    }
    
}
```
