---
layout: post
title: Kth Largest Element in an Array
category: Leetcode
---

* content
{:toc}

## Kth Largest Element in an Array

### 题目描述

> Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.
>
> For example,
>
> Given `[3,2,1,5,6,4]` and `k = 2`, return `5`.
>
> Note: 
> You may assume `k` is always valid, `1 ≤ k ≤ array's` length.

### 解法

代码如下:

    public static int findKthLargest( int[] nums, int k ) {
        return helper( nums, 0, nums.length-1, k );
    }

    private static int helper( int[] nums, int low, int high, int k ) {
        int pivot = nums[low];
        int start = low, end = high;
        while( start < end ) {
            while( start < end && nums[end] >= pivot ) --end;
            nums[start] = nums[end];
            while( start < end && nums[start] <= pivot ) ++start;
            nums[end] = nums[start];
        }
        nums[start] = pivot;
        if( k - 1 == high - start )
            return pivot;
        else if( k - 1 > high - start )
            return helper( nums, low, start-1, k-(high-start+1) );
        else
            return helper( nums, start+1, high, k );
    }

思考过程: `分治法+快排`.

借助快速排序的思想, 一次快速排序的结果就是把某个元素放置到对应的位置, 其左边的值都小于等于该元素, 其右边的值都大于等于该元素. 在快速排序的基础上配合分治法.

一次快速排序后 :

* 其右边的元素个数刚好等于`k-1`个, 则该元素就是第`k`大的元素
* 其右边的元素个数少于`k-1`个, 则第`k`大的元素在左部分
* 其右边的元素个数多于`k-1`个, 则第`k`大的元素在右部分

时空复杂度: 时间复杂度是`θ(nlogn)`, 空间复杂度是`θ(logn)`