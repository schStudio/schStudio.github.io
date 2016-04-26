---
layout: post
title: Add Digits
category: Leetcode
---

* content
{:toc}

## Add Digits

### 题目描述

> Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.
> 
> For example:
> 
> Given `num = 38`, the process is like: `3 + 8 = 11`, `1 + 1 = 2`. Since `2` has only one digit, return it.
> 
> Follow up:
> Could you do it without any loop/recursion in `O(1)` runtime?


### 解法

代码如下:

    public static int addDigits( int num ) {
        return 1 + ( num - 1 ) % 9;
    }

思考过程: 具体介绍请看[Digital root--wiki](https://en.wikipedia.org/wiki/Digital_root#Congruence_formula)

时空复杂度: 时间复杂度是`O(1)`, 空间复杂度是`O(1)`