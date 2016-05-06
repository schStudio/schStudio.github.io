---
layout: post
title: First Missing Positive
category: Leetcode
---

* content
{:toc}

## First Missing Positive

### 题目描述

> Given an unsorted integer array, find the first missing positive integer.
> 
> For example,
> 
> Given `[1,2,0]` return `3`,
> 
> and `[3,4,-1,1]` return `2`.
> 
> Your algorithm should run in `O(n)` time and uses constant space.

### 解法

代码如下:

    public static int firstMissingPositive(int[] nums) {
        int n = nums.length;
        for(int i = 0; i < n; i++)
            if(nums[i] > 0 && nums[i] <= n && nums[i] != nums[nums[i] - 1]) {
                swap(nums, i, nums[i] - 1);
                i--;
            }
        for(int i = 0; i < n; i++)
            if(nums[i] != i + 1)
                return i + 1;
        return n + 1;
    }
    private static void swap(int[] nums, int i, int j) {
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }

思考过程: 算法参考自[这里](https://leetcode.com/discuss/24013/my-short-c-solution-o-1-space-and-o-n-time)。

每次把元素放在正确的位置，也就是使得满足`nums[i] = i + 1`。

最后遍历数组查找第一次出现不满足条件的位置，该位置就是结果。

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`