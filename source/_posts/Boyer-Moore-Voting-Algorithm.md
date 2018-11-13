---
title: Boyer-Moore Voting Algorithm
date: 2018-11-10 11:45:02
tags: 算法
categories: 算法
---

# Boyer-Moore Voting Algorithm 摩尔投票算法

## 算法思想

对于一个数组nums，寻找出现次数最多的数。  
如果我们遍历这个数组，那么对于每一个当前的数currentNum，接下来的遍历可以采用投票的方式，即如果接下来的数和currentNum相等，则票数+1，不相等则-1。  
显然票数大于0的数是出现次数最多的数。

## 示例

题目出自Leetcode 169。  
Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.  

You may assume that the array is non-empty and the majority element always exist in the array.  

Example 1:  

Input: [3,2,3]  
Output: 3  
Example 2:  

Input: [2,2,1,1,1,2,2]  
Output: 2  

我们采用摩尔投票法进行解决：  
首先可以确定超过一半的元素且最多的必定只有一个，因此我们设置一个候选数candidate和对应的投票数count。  
算法实现对应的伪代码：  
<pre>
<code>
candidate,count=0
遍历数组  
    对于每个数组中元素num：
        if count==0
        candidate=num;
        count+=num==candidate?1:-1;
</code>
</pre>

对应的java代码：  
<pre><code>
class Solution {
    public int majorityElement(int[] nums) {
        int count = 0;
        Integer candidate = null;

        for (int num : nums) {
            if (count == 0) {
                candidate = num;
            }
            count += (num == candidate) ? 1 : -1;
        }

        return candidate;
    }
}
</code></pre>

## 改进算法

题目出自Leetcode 229:  
Given an integer array of size n, find all elements that appear more than ⌊ n/3 ⌋ times.  

Note: The algorithm should run in linear time and in O(1) space.  

Example 1:  

Input: [3,2,3]  
Output: [3]  
Example 2:  

Input: [1,1,1,3,3,2,2,2]  
Output: [1,2]  

从思路上来讲，我们可以根据“出现次数多余n/k”来判断最多有多少个数。本题可见最多为两个数，最少为1个。  
所以同样利用投票算法的思维可以写出：  
<pre><code>
public class MajorityElementII {
    public List< Integer > majorityElement(int[] nums) {
        int candidate1=0;
        int candidate2=0;
        int count1=0;
        int count2=0;
        for(int num:nums){
            if(num==candidate1){
                //匹配candidate1，投票++
                count1++;
            }else if(num==candidate2){
                //匹配candidate2，投票++
                count2++;
            }else if(count1==0){
                //不匹配且candidate1投票数为0
                candidate1=num;
                count1=1;
            }else if(count2==0){
                //不匹配且candidate2投票数为0
                candidate2=num;
                count2=1;
            }else{
                //不匹配且candidate1、2投票数均不为0
                count1--;
                count2--;
            }
        }
        List < Integer > res=new ArrayList<>();
        count1=0;
        count2=0;
        for(int num:nums){
            if(num==candidate1){
                count1++;
            }
            if(num==candidate2){
                count2++;
            }
        }
        int length=nums.length;
        if(count1>length/3.0){
            res.add(candidate1);
        }
        if(count2>length/3.0){
            if(candidate1!=candidate2){
                res.add(candidate2);
            }
        }
        return res;
    }
}

</pre></code>

这里在初始化candidate后要考虑数组中数是否和初始化值相等的问题。

## 算法证明

严格证明在这里不做，这里只是逻辑上的解释。当一个candidate的投票票数count为0时，当前的指针指向的数（若不为candidate），那么两个数的投票数都默认为0。此时算法默认取当前指针的值为candidate。  
更普遍的讲，每次取candidate时都是当前已遍历的数中票数（出现次数）最高的。所以遍历所有就是所有数中出现次数最高的。  
时间复杂度为O(n)，空间复杂度为O(1)。