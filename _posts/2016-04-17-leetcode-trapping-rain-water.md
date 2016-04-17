---
layout: post
title: Trapping Rain Water
category: Leetcode
---

* content
{:toc}

## Trapping Rain Water

### 题目描述

> Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.
>
> For example, 
>
> ![rainwatertrap](http://articles.leetcode.com/wp-content/uploads/2012/08/rainwatertrap.png)
>
> Given `[0,1,0,2,1,0,1,3,2,1,2,1]`, return `6`.

### 解法

代码如下:

    public static int trap( int[] nums ) {
        int low = 0, high = nums.length-1, sum = 0;
        int maxLow = low, maxHigh = high;
        while( low < high ) {
            if( nums[low] < nums[high] ) {
                if( nums[low] > nums[maxLow] ) maxLow = low;
                else sum += nums[maxLow] - nums[low];
                ++low;
            } else {
                if( nums[high] > nums[maxHigh] ) maxHigh = high;
                else sum += nums[maxHigh] - nums[high];
                --high;
            }
        }
        return sum;
    }

思考过程: 算法参考自[这里](https://leetcode.com/discuss/16171/sharing-my-simple-c-code-o-n-time-o-1-space).

首先把整个数组看成一个容器, 左边界就是数组头, 右边界就是数组尾, 每次都从较小边界往里缩进, 缩进过程中累计填充的水量, 直到边界重合为止. 在缩进的过程中要注意更新边界.

* `maxLow` : 左边界
* `maxHigh` : 右边界
* `low` : 左边界缩进指针
* `high` : 右边界缩进指针

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`