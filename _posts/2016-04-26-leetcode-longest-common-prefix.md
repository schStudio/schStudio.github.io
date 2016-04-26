---
layout: post
title: Longest Common Prefix
category: Leetcode
---

* content
{:toc}

## Longest Common Prefix

### 题目描述

> Write a function to find the longest common prefix string amongst an array of strings.


### 解法

代码如下:

    public static String longestCommonPrefix( String[] strs ) {
        if( strs == null || strs.length == 0 ) return "";
        if( strs.length == 1 ) return strs[0];
        int pos = 0;
        while( true ) {
            for( int i = 1; i < strs.length; ++i )
                if( strs[i-1].length() <= pos || strs[i].length() <= pos || 
                    strs[i].charAt( pos ) != strs[i-1].charAt( pos ) )
                    return strs[0].substring( 0, pos );
            ++pos;
        }
    }

思考过程: 

从位置的角度出发, 使用一个变量记录当前最长前缀位置, 对于每一个字符串中的当前最长前缀位置进行比较, 如果出现不等或出现字符串长度不足, 那么当前位置就是结果, 否则进入下一个位置.

注意该算法必须在拥有2个及以上的字符串数组中才能使用, 所以一开始进行边界情况的处理.

时空复杂度: 时间复杂度是`O( min(strs[i].length()) )`, 空间复杂度是`O(1)`