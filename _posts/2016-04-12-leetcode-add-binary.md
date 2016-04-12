---
layout: post
title: Add Binary
category: Leetcode
---

* content
{:toc}

## Add Binary

### 题目描述

> Given two binary strings, return their sum (also a binary string).
>
> For example,
>
> `a = "11"`
>
> `b = "1"`
>
> Return `"100"`.

### 解法

代码如下:

    public static String addBinary( String a, String b ) {
        StringBuilder sb = new StringBuilder();
        int carry = 0;
        for( int i = a.length()-1, j = b.length()-1; i >= 0 || j >= 0; --i, --j ) {
            int val1 = i >= 0 ? a.charAt(i)-'0' : 0;
            int val2 = j >= 0 ? b.charAt(j)-'0' : 0;
            int sum = val1 + val2 + carry;
            carry = sum / 2;
            sb.append( (char)( (sum%2) + '0' ) );
        }
        if( carry != 0 )
            sb.append( (char)( carry + '0' ) );
        return sb.reverse().toString();
    }

思考过程: 处理过程与[这里](http://schstudio.github.io./2016/04/11/leetcode-add-two-numbers/)相似, 重点在于学习代码的简洁性.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`
