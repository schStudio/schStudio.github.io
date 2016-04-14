---
layout: post
title: Happy Number
category: Leetcode
---

* content
{:toc}

## Happy Number

### 题目描述

> Write an algorithm to determine if a number is "happy".
>
> A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.
>
> Example: `19` is a happy number
>
> * 1<sup>2</sup> + 9<sup>2</sup> = 82
>
> * 8<sup>2</sup> + 2<sup>2</sup> = 68
> 
> * 6<sup>2</sup> + 8<sup>2</sup> = 100
> 
> * 1<sup>2</sup> + 0<sup>2</sup> + 0<sup>2</sup> = 1

### 解法

代码如下:

    public static boolean isHappy( int n ) {
        int slow = n, fast = n;
        do {
            slow = next( slow );
            fast = next( fast );
            fast = next( fast );
        } while( slow != fast );
        return slow == 1;
    }
    private static int next( int n ) {
        int sum = 0;
        while( n != 0 ) {
            int mod = n % 10;
            sum += mod * mod;
            n /= 10;
        }
        return sum;
    }

思考过程: 根据题意, 可以知道经过不断的处理, 一定会形成一个环, 要么是一个很多元素的环(非`Happy Number`), 要么是一个元素的环(`Happy Number`). 所以这道题可以使用快慢指针来处理. 算法参考自[这里](https://leetcode.com/discuss/33055/my-solution-in-c-o-1-space-and-no-magic-math-property-involved)

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`