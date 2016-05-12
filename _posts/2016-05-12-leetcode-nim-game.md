---
layout: post
title: Nim Game
category: Leetcode
---

* content
{:toc}

## Nim Game

### 题目描述

> You are playing the following Nim Game with your friend: There is a heap of stones on the table, each time one of you take turns to remove `1 to 3` stones. The one who removes the last stone will be the winner. You will take the first turn to remove the stones.
> 
> Both of you are very clever and have optimal strategies for the game. Write a function to determine whether you can win the game given the number of stones in the heap.
> 
> For example, if there are `4` stones in the heap, then you will never win the game: no matter `1`, `2`, or `3` stones you remove, the last stone will always be removed by your friend.

### 解法

代码如下:

    public static boolean canWinNim(int n) {
        return n % 4 != 0;
    }

思考过程: 

首先，得到数字`4`的一方肯定输，就像例子所解释的那样。

继续延伸思考，发现得到数字`8`的一方肯定输。

如此思考便会发现只要是数字4的倍数就肯定输了。

这样的思考就是通过样例来发现规律，是一种不错的思维方式。

时空复杂度: 时间复杂度是`O(1)`, 空间复杂度是`O(1)`