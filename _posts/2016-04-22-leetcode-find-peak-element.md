---
layout: post
title: Find Peak Element
category: Leetcode
---

* content
{:toc}

## Find Peak Element

### 题目描述

> A peak element is an element that is greater than its neighbors.
>
> Given an input array where `num[i] ≠ num[i+1]`, find a peak element and return its index.
>
> The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.
>
> You may imagine that `num[-1] = num[n] = -∞`.
>
> For example, in array `[1, 2, 3, 1]`, `3` is a peak element and your function should return the index number `2`.
>
> Note:
> Your solution should be in logarithmic complexity.

### 解法

代码如下:

    public static int findPeakElement( int[] nums ) {
        int low = 0, high = nums.length-1;
        while( low < high ) {
            int mid1 = low + ( high - low ) / 2;
            int mid2 = mid1 + 1;
            if( nums[mid1] < nums[mid2] )
                low = mid2;
            else
                high = mid1;
        }
        return low;
    }

思考过程: `二分搜索`.

给出以下两个指针的定义:

* `low` : 搜索范围的头部, 最终该指针指向`Peak Element`
* `high` : 搜索范围的尾部

只要搜索范围内还有2个及以上的元素, 就要不断搜索, 直到最后`low`指针指向`Peak Element`.

每次找到中间两个值`mid1`和`mid2`, 如果

* `nums[mid1] < nums[mid2]` : 右边肯定存在`Peak Element`, `low`指针右移
* `nums[mid1] > nums[mid2]` : 左边肯定存在`Peak Element`, `high`指针左移

时空复杂度: 时间复杂度是`O(logn)`, 空间复杂度是`O(1)`