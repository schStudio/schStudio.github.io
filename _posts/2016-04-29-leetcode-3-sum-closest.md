---
layout: post
title: 3Sum Closest
category: Leetcode
---

* content
{:toc}

## 3Sum Closest

### 题目描述

> Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.
>
    For example, given array S = {-1 2 1 -4}, and target = 1.
>
    The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

### 解法

代码如下:

    public static int threeSumClosest( int[] nums, int target ) {
        Arrays.sort( nums );
        int n = nums.length;
        int res = target, gap = Integer.MAX_VALUE;
        for( int i = 0; i < n; ++i ) {
            while( i != 0 && i < n && nums[i] == nums[i-1] ) ++i;
            if( i == n ) break;
            int twoTarget = target - nums[i];
            int low = i + 1, high = n - 1;
            while( low < high ) {
                int sum = nums[low] + nums[high];
                if( Math.abs( sum - twoTarget ) < gap ) {
                    res = sum + nums[i];
                    gap = Math.abs( sum - twoTarget );
                }
                if( sum < twoTarget )
                    ++low;
                else
                    --high;
            }
        }
        return res;
    }

思考过程: 算法思想与[3 Sum](http://schstudio.github.io./2016/04/15/leetcode-2(3)(4)-sum/#sum)是一样的.

区别在于在遍历过程中的判断标准不一样, `3 Sum`的判断标准是两值相加相等, 这里的判断标准是两值相加间隔变小.

时空复杂度: 时间复杂度是`O(n^2)`, 空间复杂度是`O(1)`