---
layout: post
title: Maximum Product Subarray
category: Leetcode
---

* content
{:toc}

## Maximum Product Subarray

### 题目描述

>Find the contiguous subarray within an array (containing at least one number) which has the largest product.
>
>For example, given the array `[2,3,-2,4]`,
>the contiguous subarray `[2,3]` has the largest product = `6`.

### 解法

代码如下:

    public static int maxProduct( int[] nums ) {
        if( nums.length == 1 ) return nums[0];

        int maxPositive = 0;
        int minNegative = 0;
        int res = 0;
        for( int i = 0; i < nums.length; ++i ) {
            if( nums[i] > 0 ) {
                maxPositive = Math.max( maxPositive*nums[i], nums[i] );
                minNegative = Math.min( minNegative*nums[i], 0 );
            } else {
                int t = maxPositive;
                maxPositive = Math.max( minNegative*nums[i], 0 );
                minNegative = Math.min( t*nums[i], nums[i] );
            }
            res = Math.max( res, maxPositive );
        }
        return res;
    }

思考过程: `动态规划`. 考虑新加入一个元素对结果带来的影响? 假设新加入元素`nums[n]`, 那么其结果有2种可能:

1. 对原结果不产生影响: 还是取原有结果
2. 对原有结果产生影响: 结果取得更大值, 而且新结果一定包含`num[n]`

以上2个可能可以看该问题具有`最优子结构`特征. 

从上面2种可能知道, 考虑新加入元素时需要: `原结果`, `左边连续子数组正数最大值`和`左边连续子数组负数最小值`(因为新结果一定包含`num[n]`). 所以定义以下三个变量即可:

* `res`: 原结果
* `maxPositive`: 左边连续子数组正数最大值
* `minNegative`: 左边连续子数组负数最小值

三个变量的状态变化情况按照当前元素`nums[i]`为正负分别考虑即可.

> 注: `maxPositive`和`minNegative`等于`0`: 左边连续子数组无正数或无负数

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`