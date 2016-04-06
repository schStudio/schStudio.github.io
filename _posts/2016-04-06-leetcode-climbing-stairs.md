---
layout: post
title: Climbing Stairs
category: Leetcode
---

* content
{:toc}

## Climbing Stairs

### 题目描述

> You are climbing a stair case. It takes n steps to reach to the top.
>
> Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

### 解法

* 动态规划

    代码如下:

        public static int climbStairs( int n ) {
            int pre1 = 0, pre2 = 1;
            for( int i = 2; i <= n; ++i ) {
                pre2 += pre1;
                pre1 = pre2 - pre1;
            }
            return pre2;
        }

    思考过程: `动态规划`

    * `当前梯度个数` = `前1步梯度个数` + `前2步梯度个数`

	> 本质上就是`Fibonacci数列中求第n+1个数`, 所以有以下`通项公式`解法

	时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`

* 通项公式

	代码如下:

        public int climbStairs(int n) {
            return (int)(1 / Math.sqrt(5)*
                (   Math.pow((1+Math.sqrt(5))/2, n+1)-
                    Math.pow((1-Math.sqrt(5))/2, n+1) )
                );
        }

    > 有关通项公式的证明: GOOGLE, 百度一下

	时空复杂度: 时间复杂度是`O(1)`, 空间复杂度是`O(1)`