---
layout: post
title: Move Zeroes
category: Leetcode
---

* content
{:toc}

## Move Zeroes

### 题目描述

> Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.
>
> For example, given `nums = [0, 1, 0, 3, 12]`, after calling your function, nums should be `[1, 3, 12, 0, 0]`.
>
> Note:
>
> * You must do this in-place without making a copy of the array.
> * Minimize the total number of operations.

### 解法

代码如下:

    public static void moveZeros( int[] nums ) {
        int zeroHead = 0;

        int curr = 0;
        while( curr < nums.length && zeroHead < nums.length ) {
            if( nums[curr] != 0 ) {
                swap( nums, zeroHead, curr );
                ++zeroHead;
            }
            ++curr;
        }
    }

    public static void swap( int[] nums, int i, int j ) {
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }

思考过程: 处理过程类似插入排序, 但是不同的地方在于:

* 插入排序 : `[1, 2, 3, 0]`处理`元素0`的时候, 需要把`[1, 2, 3]`都往后移一位, 然后插入`元素0`
* 该算法 : `[0, 0, 0, 1]`处理`元素1`的时候, 只需要交换`第一个元素0`和`元素1`就可以了

该算法可以这样处理的原因在于前面部分元素都是一样的, 所以可以节省很多移动的操作

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`
