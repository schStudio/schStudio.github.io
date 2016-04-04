---
layout: post
title: Maximum Subarray
category: Leetcode
---

* content
{:toc}

## Maximum Subarray

### 题目描述

> Find the contiguous subarray within an array (containing at least one number) which has the largest sum.
>
> For example, given the array `[−2,1,−3,4,−1,2,1,−5,4]`,
> the contiguous subarray `[4,−1,2,1]` has the largest `sum = 6`.
>
> More practice:
> 
> If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.

### 解法

* 分治法

    代码如下:

        public static int maxSubArrayDivideAndConquer( int[] nums ) {
            return helper( nums, 0, nums.length-1 );
        }
        private static int helper( int[] nums, int left, int right ) {
            if( left >= right ) return nums[left];

            int mid = left + (right-left)/2;
            int leftMax = helper( nums, left, mid-1 );
            int rightMax = helper( nums, mid+1, right );

            // midLeftMax: mid左边连续部分最大值
            // midRightMax: mid右边连续部分最大值
            int t = 0, midLeftMax = 0, midRightMax = 0;

            for( int i = mid-1; i >= left; --i ) {
                t += nums[i];
                if( t > midLeftMax ) midLeftMax = t;
            }
            t = 0;
            for( int i = mid+1; i <= right; ++i ) {
                t += nums[i];
                if( t > midRightMax ) midRightMax = t;
            }

            int midMax = midLeftMax + nums[mid] + midRightMax;
            return Math.max( Math.max( leftMax, rightMax ), midMax );
        }

    思考过程: `分治法`. 求某一个范围的`Max Subarray`, 从范围中间切成2部分, 其结果有三种可能

    1. `Max Subarray`在左半部分的`Max Subarray`
    2. `Max Subarray`在右半部分的`Max Subarray`
    3. `Max Subarray`在`中间元素`联合`左右半部分`的`Max Subarray`

    时空复杂度: 时间复杂度是`O(nlogn)`, 空间复杂度是`O(logn)`

* 动态规划

    代码如下:

        public static int maxSubArray( int[] nums ) {
            if( nums.length == 1 ) return nums[0];

            int max = nums[0], preSum = nums[0];
            for( int i = 1; i < nums.length; ++i ) {
                preSum = Math.max( preSum+nums[i], nums[i] );
                max = Math.max( max, preSum );
            }
            return max;
        }

    思考过程: `动态规划`. 考虑新加入一个元素对结果带来的影响? 假设新加入元素`num[n]`, 那么其结果有2种可能:

    1. 对原结果不产生影响: 还是取原有结果
    2. 对原有结果产生影响: 结果取得更大值, 而且新结果一定包含`num[n]`

	以上2个可能可以看该问题具有`最优子结构`特征. 

    从上面2种可能知道, 考虑新加入元素时需要: `原结果`, `左边连续子数组最大值`(因为新结果一定包含`num[n]`). 所以定义以下两个变量即可:

	* `max`: 原结果
	* `preSum`: 左边连续子数组最大值

	接下来考虑这两个变量的变化:
    
	* `下一个preSum`: 因为`preSum`是连续的, 那么一定有`num[i]`, 如果加上`preSum`可以取得更大值, 那么就取`preSum`, 否则不取
	* `下一个max`: 保留`原结果`或更换为`新结果preSum`

    时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`