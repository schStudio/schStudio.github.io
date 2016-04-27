---
layout: post
title: Product of Array Except Self
category: Leetcode
---

* content
{:toc}

## Product of Array Except Self

### 题目描述

> Given an array of `n` integers where `n > 1`, nums, return an array output such that `output[i]` is equal to the product of all the elements of nums except `nums[i]`.
> 
> Solve it without division and in `O(n)`.
> 
> For example, given `[1,2,3,4]`, return `[24,12,8,6]`.
> 
> Follow up:
> Could you solve it with constant space complexity? (Note: The output array does not count as extra space for the purpose of space complexity analysis.)

### 解法

代码如下:

    public static int[] productExceptSelf( int[] nums ) {
        if( nums == null || nums.length == 0 || nums.length == 1 )
            return nums;

        int[] res = new int[nums.length];
        Arrays.fill( res, 1 );
        int n = nums.length, left = nums[0], right = nums[n-1];
        for( int i = 1; i < n; ++i ) {
            res[i] *= left;
            res[n-1-i] *= right;
            left *= nums[i];
            right *= nums[n-1-i];
        }
        return res;
    }

思考过程: 

考虑一个位置的结果, 等于该位置左边的乘积, 乘以该位置右边的乘积.

定义两个变量:

* `left` : 排除当前位置的左边乘积
* `right` : 排除当前位置的右边乘积

因为左边乘积和右边乘积是以累积的形式, 所以一边遍历一边累积.

再思考发现左边累积和右边累积的运算是对称的, 而且彼此之间不影响, 所以可以在一次遍历中同时运算.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`