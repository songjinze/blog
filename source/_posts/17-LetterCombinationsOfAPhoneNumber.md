---
title: 17 LetterCombinationsOfAPhoneNumber
tags: LeetCode
categories: LeetCode
date: 2019-02-08 22:44:36
---


## 题目描述

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.  

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.  

{% asset_img 描述.PNG 描述 %}  

Example:  

Input: "23"  
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].  

## 解题思路

采用回溯(Backtracing)算法，
> Backtracking is an algorithm for finding all solutions by exploring all potential candidates. If the solution candidate turns to be not a solution (or at least not the last one), backtracking algorithm discards it by making some changes on the previous step, i.e. backtracks and then try again.  

其实也可以看成是一种广度优先遍历。
本题采用递归，递归保存数字可能的字母构成的字符串。

## 源码

```java
class Solution {
    private Map<String,String> phone=new HashMap<String,String>(){{
        put("2","abc");
        put("3","def");
        put("4","ghi");
        put("5","jkl");
        put("6","mno");
        put("7","pqrs");
        put("8","tuv");
        put("9","wxyz");
    }};
    private List<String> res=new ArrayList<>();
    private void backTracing(String cur,String next){
        if(next.length()==0){
            res.add(cur);
            return;
        }else{
            String temp=next.substring(0,1);
            String letters=phone.get(temp);
            for(int i=0;i<letters.length();i++){
                String letter=letters.substring(i,i+1);
                backTracing(cur+letter,next.substring(1));
            }
        }
    }
    public List<String> letterCombinations(String digits) {
        if(digits.length()!=0){
            backTracing("",digits);
        }
        return res;
    }
}
```
