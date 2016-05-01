---
layout: post
title: Minimum Size Subarray Sum
category: Leetcode
---

* content
{:toc}

## Minimum Size Subarray Sum

### 题目描述

> Given an array of `n` positive integers and a positive integer `s`, find the minimal length of a subarray of which the `sum ≥ s`. If there isn't one, return `0` instead.
> 
> For example, given the array `[2,3,1,2,4,3]` and `s = 7`,
> 
> the subarray `[4,3]` has the minimal length under the problem constraint.

### 解法

代码如下:

    public static int minSubArrayLen(int s, int[] nums) {
        int preSum = 0, preLen = 0; 
        int res = Integer.MAX_VALUE;
        for(int i = 0; i < nums.length; i++) {
            preSum += nums[i];
            preLen++;
            while( preSum - nums[i - preLen + 1] >= s ) {
                preSum -= nums[i - preLen + 1];
                preLen--;
            }
            if(preSum >= s)
                res = Math.min(res, preLen);
        }
        return res == Integer.MAX_VALUE ? 0 : res;
    }

思考过程: `动态规划`.

首先思考最后一个元素对结果带来的影响: 假设元素`[n]`是最后一个元素, 那么满足条件`sum([i...n]) >= s && sum([i+1...n]) < s`的子数组`[i...n]`的长度`n-i+1`就是可能的结果, 如果该长度小于当前的长度, 那么更新结果, 否则不管.

根据上面的分析, 知道当前数组的结果依赖于子数组的结果, 也就具有最优子结构性质. 那么定义以下的变量:

* `preSum`: 当前数组最右端子数组的和, 刚好满足条件`preSum >= s`
* `preLen`: 当前数组最右端子数组的长度, 取值依赖于`preSum`
* `res`: 当前数组的结果

对于数组`[0...i-1]`新加入的元素`[i]`, 做出以下处理:

1. `preSum`加上新元素`[i]`, `preLen`也加上`1`
2. 向右缩进当前数组最右端子数组的和`preSum`, 使得刚好满足条件`preSum >= s`;
3. 更新当前数组的结果

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`