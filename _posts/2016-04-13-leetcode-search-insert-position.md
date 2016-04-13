---
layout: post
title: Search Insert Position
category: Leetcode
---

* content
{:toc}

## Search Insert Position

### 题目描述

> Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.
>
> You may assume no duplicates in the array.
>
> Here are few examples.
>
> `[1,3,5,6]`, `5 → 2`
>
> `[1,3,5,6]`, `2 → 1`
>
> `[1,3,5,6]`, `7 → 4`
>
> `[1,3,5,6]`, `0 → 0`

### 解法

代码如下:

    public static int searchInsert( int[] nums, int target ) {
        int low = 0, high = nums.length-1;
        while( low <= high ) {
            int mid = low + (high-low)/2;
            if( nums[mid] == target )
                return mid;
            else if( nums[mid] < target )
                low = mid + 1;
            else
                high = mid - 1;
        }
        return low;
    }

思考过程: `二分搜索`. 二分搜索谁都会写, 但是不一定都能写对, 因为二分搜索有很多边界条件让人琢磨不透. 这道题要求找不到对应的目标值就返回插入的位置, 那么可以从两个角度去思考:

* `low` : `low`在循环中一定指向小于目标值, 跳出循环外就是刚好大于目标值, 也就是插入位置
* `high` : `high`在循环中一定指向大于目标值, 跳出循环外就是刚好小于目标值, 也就是插入位置减1

时空复杂度: 时间复杂度是`O(logn)`, 空间复杂度是`O(1)`
