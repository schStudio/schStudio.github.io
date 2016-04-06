---
layout: post
title: Find Minimum in Rotated Sorted Array(II)
category: Leetcode
---

* content
{:toc}

## Find Minimum in Rotated Sorted Array

### 题目描述

> Suppose a sorted array is rotated at some pivot unknown to you beforehand.
>
> (i.e., `0 1 2 4 5 6 7` might become `4 5 6 7 0 1 2`).
>
> Find the minimum element.
>
> You may assume no duplicate exists in the array.

### 解法

代码如下:

    public static int findMin( int[] nums ) {
        int left = 0, right = nums.length-1;
        while( left < right ) {
            int mid = left + (right-left)/2;
            if( nums[left] < nums[right] )
                return nums[left];
            else if( nums[mid] >= nums[left]  )
                left = mid + 1;
            else
                right = mid;
        }
        return nums[left];
    }

思考过程: `二分搜索`. 根据题意, 知道一个有序的数组的翻转点有3种情况:

* 翻转点在中点左边
* 翻转点在中点
* 翻转点在中点右边

分别画出这3种情况的大概图示进行分析即可.

> 需要注意的地方: 一定考虑清楚`left`, `right`的状态变化边界条件.

时空复杂度: 时间复杂度是`O(logn)`, 空间复杂度是`O(1)`

- - -

## Find Minimum in Rotated Sorted Array II

### 题目描述

> > Follow up for "Find Minimum in Rotated Sorted Array":
> >
> > What if duplicates are allowed?
>
> Would this affect the run-time complexity? How and why?
>
> Suppose a sorted array is rotated at some pivot unknown to you beforehand.
>
> (i.e., `0 1 2 4 5 6 7` might become `4 5 6 7 0 1 2`).
>
> Find the minimum element.
>
> The array may contain duplicates.

### 解法

代码如下:

    public static int findMin( int[] nums ) {
        int left = 0, right = nums.length-1;
        while( left < right ) {
            int mid = left + (right-left)/2;
            if( nums[mid] > nums[right] )
                left = mid + 1;
            else if( nums[mid] < nums[right]  )
                right = mid;
            else			// 不确定最小值在左边还是右边
                --right;
        }
        return nums[left];
    }

思考过程: `二分搜索+线性扫描`. 根据题意, 这道题的考虑出发点是中点与右边界之间的关系:

* 中点大于右边界 : 最小值肯定在右边
* 中点小于右边界 : 最小值肯定在左边
* 中点等于右边界 : 最小值可能在左边, 可能在右边

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`

> 最坏情况下整个数组都是相同元素, 那么该算法会进行线性扫描