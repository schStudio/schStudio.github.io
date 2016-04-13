---
layout: post
title: Reverse Integer
category: Leetcode
---

* content
{:toc}

## Reverse Integer

### 题目描述

> Reverse digits of an integer.
>
> Example1: `x = 123`, return `321`
>
> Example2: `x = -123`, return `-321`

### 解法

代码如下:

    public static int reverse( int x ) {
        if( x == Integer.MIN_VALUE ) return 0;
        int sign = x > 0 ? 1 : -1;
        x = Math.abs( x );

        int res = 0;
        while( x != 0 ) {
            if( (res > Integer.MAX_VALUE/10) || 
                    (res==Integer.MAX_VALUE/10 && x%10>=7) )
                return 0;
            res = res*10 + x%10;
            x /= 10;
        }

        return sign*res;
    }

思考过程: 先获取符号, 然后取绝对值进行转换, 需要注意的问题:

* 因为使用绝对值, 所以要单独考虑`Integer.MIN_VALUE`
* 考虑溢出

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`