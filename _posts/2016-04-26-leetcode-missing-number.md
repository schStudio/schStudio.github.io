---
layout: post
title: Missing Number
category: Leetcode
---

* content
{:toc}

## Missing Number

### 题目描述

> Given an array containing n distinct numbers taken from `0, 1, 2, ..., n`, find the one that is missing from the array.
> 
> For example,
> 
> Given `nums = [0, 1, 3]` return `2`.
> 
> Note:
> 
> Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?


### 解法

代码如下:

    public static int missingNumber( int[] nums ) {
        int sum = 0, n = nums.length;
        for( int i = 0; i < n; ++i )
            sum += nums[i];
        return n * ( n + 1 ) / 2 - sum;
    }

思考过程: 

根据条件, 已知数组中丢失的元素是数值范围`[0...n]`中的某一个值, 那么用该范围所有元素的和减去丢失后所有元素的和就是丢失的值.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`