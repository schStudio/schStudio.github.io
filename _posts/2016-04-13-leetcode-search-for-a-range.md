---
layout: post
title: Search for a Range
category: Leetcode
---

* content
{:toc}

## Search for a Range

### 题目描述

> Given a sorted array of integers, find the starting and ending position of a given target value.
>
> Your algorithm's runtime complexity must be in the order of `O(log n)`.
>
> If the target is not found in the array, return `[-1, -1]`.
>
> For example,
>
> Given `[5, 7, 7, 8, 8, 10]` and target value `8`,
>
> return `[3, 4]`.

### 解法

代码如下:

    public static int[] searchRange( int[] nums, int target ) {
        return new int[] {
                searchLeft( nums, target ),
                searchRight( nums, target )
        };
    }

    private static int searchLeft( int[] nums, int target ) {
        int low = 0, high = nums.length-1;
        while( low <= high ) {
            int mid = low + (high-low)/2;
            if( nums[mid] < target )
                low = mid + 1;
            else
                high = mid - 1;
        }
        if( low >= nums.length ) return -1;
        return nums[low] == target ? low : -1;
    }

    private static int searchRight( int[] nums, int target ) {
        int low = 0, high = nums.length-1;
        while( low <= high ) {
            int mid = low + (high-low)/2;
            if( nums[mid] <= target )
                low = mid + 1;
            else
                high = mid - 1;
        }
        if( high < 0 ) return -1;
        return nums[high] == target ? high : -1;
    }

思考过程: `二分搜索`. 该题与[Search Insert Position](http://schstudio.github.io./2016/04/13/leetcode-search-insert-position/)一样, 都是考察对二分搜索边界条件的理解, 下面给出`searchLeft`与`searchRight`的解释:

* `searchLeft` : `low`在循环中一定指向小于目标值, 跳出循环外就是刚好大于等于目标值, 但是要先判断是否存在目标值, 存在的情况才是范围的最左边, 不存在就是大于目标值.
* `searchRight` : `high`在循环中一定指向大于目标值, 跳出循环外就是刚好小于等于目标值, 一样要先判断是否存在目标值, 存在的情况才是范围的右边, 不存在就是小于目标值.

时空复杂度: 时间复杂度是`O(logn)`, 空间复杂度是`O(1)`
