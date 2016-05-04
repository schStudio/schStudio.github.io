---
layout: post
title: ZigZag Conversion
category: Leetcode
---

* content
{:toc}

## ZigZag Conversion

### 题目描述

> The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)
>
    P   A   H   N
    A P L S I I G
    Y   I   R
>
> And then read line by line: `"PAHNAPLSIIGYIR"`
> Write the code that will take a string and make this conversion given a number of rows:
>
> `string convert(string text, int nRows);`
>
> `convert("PAYPALISHIRING", 3)` should return `"PAHNAPLSIIGYIR"`.

### 解法

代码如下:

    public static String convert(String s, int numRows) {
        if(numRows == 1) return s;
        StringBuilder res = new StringBuilder();
        int gap = 2 * (numRows - 1);
        for (int i = 0; i < numRows; i++) {
            int j = i;
            if(j < s.length())
                res.append(s.charAt(j));
            while(j < s.length()) {
                j += gap - 2 * i == 0 ? gap : gap - 2 * i;
                if(j < s.length())
                    res.append(s.charAt(j));
                j += 2 * i == 0 ? gap : 2 * i;
                if(j < s.length())
                    res.append(s.charAt(j));
            }
        }
        return res.toString();
    }

思考过程:

首先找到规律:

* 第一行字母位置: `j + 2*(numRows - 1)`
* 第二行到倒数第二行字母位置是: `j + 2*(numRows - i - 1)` 和 `j + 2*i`
* 最后一行字母位置: `j + 2*(numRows - 1)`

其中`j`表示当前位置.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`
