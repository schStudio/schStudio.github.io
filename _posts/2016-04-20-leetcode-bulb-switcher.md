---
layout: post
title: Bulb Switcher
category: Leetcode
---

* content
{:toc}

## Bulb Switcher

### 题目描述

> There are n bulbs that are initially off. You first turn on all the bulbs. Then, you turn off every second bulb. On the third round, you toggle every third bulb (turning on if it's off or turning off if it's on). For the ith round, you toggle every i bulb. For the nth round, you only toggle the last bulb. Find how many bulbs are on after n rounds.
>
> Example:
>
    Given n = 3. 
>
    At first, the three bulbs are [off, off, off].
    After first round, the three bulbs are [on, on, on].
    After second round, the three bulbs are [on, off, on].
    After third round, the three bulbs are [on, off, off]. 
>
> So you should return 1, because there is only one bulb is on.

### 解法

代码如下:

	public static int bulbSwitch( int n ) {
	    return (int)( Math.sqrt( n ) );
	}

思考过程: 该算法参考自[这里](https://leetcode.com/discuss/75014/math-solution)

根据题意知道一个`bulb`如果最终是`on`的状态, 那么`bulb`被`switch`了奇数次.

接下来题目说在d回合的时候会对第`[d,2d,3d...nd]`个的`bulb`切换, 这告诉我们d是这些`bulb`的因数,所以问题转化为: 因数个数为奇数的`bulb`最终状态为`on`.

最后从数字的特征出发, 一个数的因数几乎所有情况下都是成双成对出现的, 例如`12: 1*12, 2*6, 3*4`有这样3对出现, 但是有例外, 例如`16: 1*16, 2*8, 4*4`, 这里的4就不是成对出现了, 也就是16的因数个数是奇数个, 所以平方数的因数个数为奇数个.

因此问题又可以转化为: 求出所有`bulb`编号为平方数的个数. 这样一来求出`sqrt(n)`范围内的个数就是结果了.

时空复杂度: 时间复杂度是`O(Math.sqrt)`, 空间复杂度是`O(1)`