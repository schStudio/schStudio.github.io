---
layout: post
title: Implement strStr()
category: Leetcode
---

* content
{:toc}

## Implement strStr()

### 题目描述

> Implement strStr().
>
> Returns the index of the first occurrence of needle in haystack, or `-1` if needle is not part of haystack.

### 解法

代码如下:

    public static int strStr( String haystack, String needle ) {
        if( haystack == null || needle == null ) return -1;
        int m = haystack.length(), n = needle.length();
        for( int i = 0; i <= m - n; ++i ) {
            int j = 0;
            for( ; j < n; ++j )
                if( haystack.charAt( i + j ) != needle.charAt( j ) )
                    break;
            if( j == n )
                return i;
        }
        return -1;
    }

思考过程: 

首先考虑两个参数字符串分别为空的情况.

再者使用两重循环遍历查找, 如果查找成功立即返回.

如果遍历过程没有返回, 说明匹配不成功, 返回`-1`.

时空复杂度: 时间复杂度是`O(m*n)`, 空间复杂度是`O(1)`