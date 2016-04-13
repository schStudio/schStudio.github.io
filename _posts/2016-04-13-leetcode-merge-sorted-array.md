---
layout: post
title: Merge Sorted Array
category: Leetcode
---

* content
{:toc}

## Merge Sorted Array

### 题目描述

> Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.
>
> Note:
>
> You may assume that nums1 has enough space (size that is greater or equal to `m + n`) to hold additional elements from nums2. The number of elements initialized in nums1 and nums2 are `m` and `n` respectively.

### 解法

代码如下:

    public static void merge( int[] nums1, int m, int[] nums2, int n ) {
        int i = m-1, j = n-1, k = m+n-1;
        while( i >= 0 || j >= 0 ) {
            if( i < 0 )
                nums1[k] = nums2[j--];
            else if( j < 0 )
                nums1[k] = nums1[i--];
            else if( nums1[i] < nums2[j] )
                nums1[k] = nums2[j--];
            else
                nums1[k] = nums1[i--];
            k--;
        }
    }

思考过程: 算法参考自[这里](https://leetcode.com/discuss/8233/this-is-my-ac-code-may-help-you). 主要的切入点是从后往前扫描两个数组, 然后把较大者放入目标数组的后面, 这样考虑的话就不需要移动元素.

时空复杂度: 时间复杂度是`O(m+n)`, 空间复杂度是`O(1)`
