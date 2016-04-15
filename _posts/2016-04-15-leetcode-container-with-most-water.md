---
layout: post
title: Container With Most Water
category: Leetcode
---

* content
{:toc}

## Container With Most Water

### 题目描述

> Given n non-negative integers `a1, a2, ..., an`, where each represents a point at coordinate `(i, ai)`. n vertical lines are drawn such that the two endpoints of line i is at `(i, ai)` and `(i, 0)`. Find two lines, which together with x-axis forms a container, such that the container contains the most water.
>
> Note: You may not slant the container.

### 解法

代码如下:

    public static int mostWater( int[] height ) {
        int res = 0;
        int low = 0, high = height.length-1;
        while( low < high ) {
            int h = Math.min( height[low], height[high] );
            res = Math.max( res, h*(high-low) );
            while( low < high && height[low] <= h ) ++low;
            while( low < high && height[high] <= h ) --high;
        }
        return res;
    }

思考过程: 算法参考自[这里](https://leetcode.com/discuss/41527/simple-and-fast-c-c-with-explanation).

一开始的想法是遍历所有的子数组, 因为容量的大小跟高度和宽度两个因素有关, 所以没有一个导向标准, 但是后来发现宽度实际上是可以作为导向标准的, 因为我们可以控制宽度从大到小变化, 所以以此作为突破口就得到上面的算法.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`