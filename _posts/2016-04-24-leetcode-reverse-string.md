---
layout: post
title: Reverse String
category: Leetcode
---

* content
{:toc}

## Reverse String

### 题目描述

> Write a function that takes a string as input and returns the string reversed.
>
> Example:
> Given `s = "hello"`, return `"olleh"`.


### 解法

代码如下:

    public String reverseString(String s) {
        char[] cstr = s.toCharArray();
        int low = 0, high = cstr.length-1;
        while( low < high ) {
            char c = cstr[low];
            cstr[low++] = cstr[high];
            cstr[high--] = c;
        }
        return new String( cstr );
    }

思考过程:

题目要求是翻转字符串, 方法是使用头尾指针交换字符.

首先头指针指向待交换字母, 然后尾指针也指向待交换字母, 最后交换头尾指针元素.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(n)`