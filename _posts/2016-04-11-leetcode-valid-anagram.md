---
layout: post
title: Valid Anagram
category: Leetcode
---

* content
{:toc}

## Valid Anagram

### 题目描述

> Given two strings s and t, write a function to determine if t is an anagram of s.
>
> For example,
> 
> `s = "anagram"`, `t = "nagaram"`, return `true`.
> 
> `s = "rat"`, `t = "car"`, return `false`.
>
> Note:
> 
> * You may assume the string contains only lowercase alphabets.

### 解法

代码如下:

    public static boolean isAnagram( String s, String t ) {

        if( s.length() != t.length() ) return false;

        int[] hash = new int[26];

        for( int i = 0; i < s.length(); ++i ) {
            ++hash[s.charAt(i)-'a'];
            --hash[t.charAt(i)-'a'];
        }
        for( int i = 0; i < 26; ++i )
            if( hash[i] != 0 ) return false;

        return true;
    }

思考过程: `哈希表`. 创建一个哈希表, 对于`字符串s`中的每一个字符, 对应的位置统计量加1, 对于字符串`t`中的每一个字符, 对应的位置统计量减1; 最后检查哈希表中每个位置是否都为0, 如果都为0, 说明为anagram, 如果有一个不为0, 说明不为anagram.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`
