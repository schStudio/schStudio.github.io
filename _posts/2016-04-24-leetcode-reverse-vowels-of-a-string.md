---
layout: post
title: Reverse Vowels of a String
category: Leetcode
---

* content
{:toc}

## Reverse Vowels of a String

### 题目描述

> Write a function that takes a string as input and reverse only the vowels of a string.
> 
> Example 1:
> 
> Given `s = "hello"`, return `"holle"`.
> 
> Example 2:
> 
> Given `s = "leetcode"`, return `"leotcede"`.


### 解法

代码如下:

    public static String reverseVowels( String s ) {
        char[] cstr = s.toCharArray();
        int low = 0, high = cstr.length-1;
        while( low < high ) {
            while( low < high && "aeiou".indexOf( Character.toLowerCase( cstr[low] ) ) < 0 ) 
                ++low;
            while( low < high && "aeiou".indexOf( Character.toLowerCase( cstr[high] ) ) < 0 ) 
                --high;
            char c = cstr[low];
            cstr[low++] = cstr[high];
            cstr[high--] = c;
        }
        return new String( cstr );
    }

思考过程:

题目要求是翻转字符串中的元音字母, 那么跟翻转字符串是一样的, 方法是使用头尾指针交换字符.

首先头指针不断移动直到指向元音字母, 然后尾指针也不断移动直到指向元音字母, 最后交换头尾指针元素.

处理过程中需要注意, 题目要求的元音字母不区分大小写.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`