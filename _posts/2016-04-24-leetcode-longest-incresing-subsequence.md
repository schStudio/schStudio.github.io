---
layout: post
title: Longest Increasing Subsequence
category: Leetcode
---

* content
{:toc}

## Longest Increasing Subsequence

### 题目描述

> Given an unsorted array of integers, find the length of longest increasing subsequence.
> 
> For example,
> 
> Given `[10, 9, 2, 5, 3, 7, 101, 18]`,
> 
> The longest increasing subsequence is `[2, 3, 7, 101]`, therefore the length is `4`. Note that there may be more than one LIS combination, it is only necessary for you to return the length.
> 
> Your algorithm should run in `O(n^2)` complexity.
> 
> Follow up: Could you improve it to `O(nlogn)` time complexity?


### 解法

`O(n^2)`代码如下:

    public static int lengthOfLIS(int[] nums) {
        int res = 0;
        // dp[i]: 上升子序列长度为i+1的边界
        int[] dp = new int[nums.length];
        Arrays.fill( dp, Integer.MAX_VALUE );
        for( int i = 0; i < nums.length; ++i ) {
            int j = 0;
            while( j < i && dp[j] < nums[i] ) ++j;
            dp[j] = nums[i];
            res = Math.max( res, j+1 );
        }
        return res;
    }

`O(nlogn)`代码如下:

    public static int lengthOfLIS(int[] nums) {
        int res = 0;
        // dp[i]: 上升子序列长度为i+1的边界
        int[] dp = new int[nums.length];
        Arrays.fill( dp, Integer.MAX_VALUE );
        for( int i = 0; i < nums.length; ++i ) {
            int j = bsearchLeft( dp, 0, i, nums[i] );
            dp[j] = nums[i];
            res = Math.max( res, j+1 );
        }
        return res;
    }

    private static int bsearchLeft( int[] nums, int low, int high, int target ) {
        while( low < high ) {
            int mid = low + ( high - low ) / 2;
            if( nums[mid] >= target )
                high = mid;
            else
                low = mid + 1;
        }
        return low;
    }

思考过程: `动态规划+二分查找`.

考虑数组`[A0, A1, A2...Ai]`的最长上升子序列长度, 如果知道`[A0, A1, A2...Ai-1]`的最长上升子序列为`[...Ak]`而且`Ai > Ak`, 那么结果就为`length([...Ak])+1`, 也就是这道题有最优子结构性质, 所以选择动态规划来解决.

上面讲到, 求当前的最长上升子序列长度, 需要知道子问题的最长上升子序列的边界值, 也就是`Ak`的值. 因此建立一个数组:

* `dp[i]` : `i`作为下标, `i+1`表示长度, `dp[i]`表示长度为`i+1`的最长上升子序列边界值

对于元素`Ai`, 我们需要更新`dp`数组的值. 例如`dp = [1, 3, 5]`, `Ai = 4`, 那么更新后变为`dp = [1, 3, 4]`, 因为出现了更小的边界值, 就需要更新掉旧有的边界值.

在更新边界值的时候, 可以采用顺序查找的方式, 也可以采用二分查找的方式.

采用二分查找的方式, 是基于以下的条件才成立的:

* `i < j ==> dp[i] < dp[j]`

这个条件的意思就是说对于同样一组元素, 最长上升子序列长度更大的边界值会更大.

由于题目要求只要长度, 那么在更新边界值的同时, 更新最长值就可以了.