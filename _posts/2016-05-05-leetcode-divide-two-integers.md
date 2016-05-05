---
layout: post
title: Divide Two Integers
category: Leetcode
---

* content
{:toc}

## Divide Two Integers

### 题目描述

> Divide two integers without using multiplication, division and mod operator.
>
> If it is overflow, return `MAX_INT`.

### 解法

代码如下:

    public static int divide(int dividend, int divisor) {
        if(dividend == Integer.MIN_VALUE && divisor == -1)
            return Integer.MAX_VALUE;
        if(divisor == 0)
            return Integer.MAX_VALUE;
        int sign = (dividend > 0 ^ divisor > 0) ? -1 : 1;
        long dvd = dividend < 0 ? -(long)dividend : dividend;
        long dvs = divisor < 0 ? -(long)(divisor) : divisor;
        int res = 0;
        while(dvd >= dvs) {
            long temp = dvs, multiple = 1;
            while(dvd >= (temp<<1)) {
                temp <<= 1;
                multiple <<= 1;
            }
            dvd -= temp;
            res += multiple;
        }
        return sign * res;
    }

思考过程: 算法参考自[这里](https://leetcode.com/discuss/38997/detailed-explained-8ms-c-solution)。

具体例子直接参考上面的链接。

主要思想是借助位移运算来实现计数。

时空复杂度: 时间复杂度是`O(不确定)`, 空间复杂度是`O(不确定)`