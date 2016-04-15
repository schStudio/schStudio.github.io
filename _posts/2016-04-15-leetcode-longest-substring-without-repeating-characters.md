---
layout: post
title: Longest Substring Without Repeating Characters
category: Leetcode
---

* content
{:toc}

## Longest Substring Without Repeating Characters

### 题目描述

> Given a string, find the length of the longest substring without repeating characters.
>
> Examples:
>
> Given `"abcabcbb"`, the answer is `"abc"`, which the length is `3`.
>
> Given `"bbbbb"`, the answer is `"b"`, with the length of `1`.
>
> Given `"pwwkew"`, the answer is `"wke"`, with the length of `3`. Note that the answer must be a substring, `"pwke"` is a subsequence and not a substring.

### 解法

代码如下:

    public static int lengthOfLongestSubstring( String s ) {
        int res = 0;
        // hash[i]表示字符i在字符串中的位置
        int[] hash = new int[256];
        // start表示无重复子串的开始位置
        int start = 0;
        Arrays.fill( hash, -1 );

        for( int i = 0; i < s.length(); ++i ) {
            char ch = s.charAt( i );
            if( hash[ch] != -1 ) {
                res = Math.max( res, i-hash[ch] );
                while( start <= hash[ch] )
                    hash[s.charAt( start++ )] = -1; 
            } else {
                res = Math.max( res, i-start+1 );
            }
            hash[ch] = i;
        }
        return res;
    }

思考过程: 对于位置`[i]`, 如果想要求出无重复串`[k...i]`, 需要知道的条件有:

* `[i-1]`位置能够构造的无重复串`[m...i-1]`

如果位置`[i]`与无重复串`[m...i-1]`中每一个字符都没有重复, 那么可以构造无重复串`[m...i]`; 如果位置`[i]`与无重复串`[m...i-1]`中某一个位置`[n]`字符一样, 那么可以构造无重复串`[n+1...i]`

综上所述, `k`的取值可能为为`m`, 也可能为`n`, 其表示的意义如下:

* `m` : 前一无重复串的开始位置
* `n` : 前面重复字符的位置

因此我们使用一个变量`start`来表示`m`, 而借助哈希表来查询`n`.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`