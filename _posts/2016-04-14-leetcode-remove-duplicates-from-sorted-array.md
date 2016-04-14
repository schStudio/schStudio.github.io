---
layout: post
title: Remove Duplicates from Sorted Array
category: Leetcode
---

* content
{:toc}

## Remove Duplicates from Sorted Array

### 题目描述

> Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.
>
> Do not allocate extra space for another array, you must do this in place with constant memory.
>
> For example,
>
> Given input array nums = `[1,1,2]`,
>
> Your function should return length = `2`, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the new length.

### 解法

代码如下:

    public static int removeDuplicates( int[] nums ) {
        if( nums.length < 2 ) return nums.length;
        int index = 0;
        for( int i = 1; i < nums.length; ++i )
            if( nums[i] != nums[index] )
                nums[++index] = nums[i];
        return index + 1;
    }

思考过程: `[0...n-1]`数组经过去重之后变为`[0...index]`(`index <= n-1`), 所以可以使用`index`变量来维护新数组的元素范围, 在这个过程中遍历数组, 不断加入非重复元素.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`