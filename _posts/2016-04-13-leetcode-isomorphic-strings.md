---
layout: post
title: Isomorphic Strings
category: Leetcode
---

* content
{:toc}

## Isomorphic Strings

### 题目描述

> Given two strings `s` and `t`, determine if they are isomorphic.
>
> Two strings are isomorphic if the characters in `s` can be replaced to get `t`.
>
> All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.
>
> For example,
>
> Given `"egg"`, `"add"`, return `true`.
>
> Given `"foo"`, `"bar"`, return `false`.
>
> Given `"paper"`, `"title"`, return `true`.
>
> Note:
>
> * You may assume both `s` and `t` have the same length.

### 解法

代码如下:

    public static boolean isIsomorphic( String s, String t ) {
        int[] m1 = new int[256];
        int[] m2 = new int[256];
        for( int i = 0; i < s.length(); ++i ) {
            char a = s.charAt( i );
            char b = t.charAt( i );
            if( m1[a] != m2[b] ) return false;
            m1[a] = i + 1;
            m2[b] = i + 1;
        }
        return true;
    }

思考过程: `哈希表`. 根据题意可以知道字符串`s`和`t`之间字符的对应关系是一对一的, 而且不能重复使用. 所以解决这两个问题也就解决了问题:

* 一对一关系 : `s`和`t`中的字符都映射到一个值, 如果值相等则建立了一对一关系
* 不重复使用 : 不重复使用可以借助位置变量`i`进行标记, 因为`i`是递增的

因此, 对于每一个位置, 取出两个字符串中的字符进行映射判断, 然后建立一对一关系, 代码中每一次都建立这种一对一关系, 即使覆盖了之前的关系也没有问题, 因为我们关注的是一对一关系和不重复使用.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(n)`
