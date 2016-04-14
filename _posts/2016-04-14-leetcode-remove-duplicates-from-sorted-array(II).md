---
layout: post
title: Remove Duplicates from Sorted Array(II)
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

- - -

## Remove Duplicates from Sorted Array II

### 题目描述

> Follow up for "Remove Duplicates":
> What if duplicates are allowed at most twice?
>
> For example,
>
> Given sorted array `nums = [1,1,1,2,2,3]`,
>
> Your function should return length = 5, with the first five elements of nums being `1, 1, 2, 2 and 3`. It doesn't matter what you leave beyond the new length.

### 解法

代码如下:

    public static int removeDuplicates( int[] nums ) {
        int k = 2;
        if( nums.length < k + 1 ) return nums.length;
        int index = k-1;
        for( int i = k; i < nums.length; ++i )
            if( nums[i] != nums[index-k+1] )
                nums[++index] = nums[i];
        return index + 1;
    }

思考过程: 题目要求可以重复2个元素, 这里我把代码修改为可以重复k个元素, 原理与上题一样.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`