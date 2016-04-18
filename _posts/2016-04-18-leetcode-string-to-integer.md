---
layout: post
title: String to Integer (atoi)
category: Leetcode
---

* content
{:toc}

## String to Integer (atoi)

### 题目描述

> Implement atoi to convert a string to an integer.
>
> Requirements for atoi:
>
> * The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.
> * The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.
> * If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.
> * If no valid conversion could be performed, a zero value is returned. If the correct value is out of the range of representable values, `INT_MAX (2147483647)` or `INT_MIN (-2147483648)` is returned.

### 解法

代码如下:

    public static int myAtoi( String str ) {
        int i = 0;
        // 跳过空白符
        while( i < str.length() && str.charAt( i ) == ' ' ) ++i;
        // 判断符号
        int sign = 1;
        if( i < str.length() && str.charAt( i ) == '+' ) { sign = 1; ++i; }
        else if( i < str.length() && str.charAt( i ) == '-' ) { sign = -1; ++i; }
        // 转化
        int res = 0;
        int maxDiv10 = Integer.MAX_VALUE / 10;
        int maxMod10 = Integer.MAX_VALUE % 10;
        while( i < str.length() && Character.isDigit(str.charAt(i)) ) {
            int val = str.charAt( i ) - '0';
            // 值溢出
            if( res > maxDiv10 || ( res == maxDiv10 && val > maxMod10 ) )
                return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
            res = res * 10 + val;
            ++i;
        }
        return sign*res;
    }

思考过程: 模拟转化过程, 但是要注意以下几个步骤:

* 跳过前置的空白符
* 判断是否有符号, 有的话要提取
* 转化过程要注意值的溢出

一开始认为正数与负数的取值范围不一样, 相差了一个值, 就是`-2147483648`, 所以认为需要对正负数分别做溢出判断.

实际上题目要求在值溢出的时候要返回溢出边界值, 而`-2147483648`本身就是溢出边界值, 所以可以把它归为值溢出的情况, 那么正数负数的取值范围就对称了, 可以简化处理过程.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`