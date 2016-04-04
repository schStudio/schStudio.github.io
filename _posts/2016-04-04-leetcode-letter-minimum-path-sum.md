---
layout: post
title: Minimum Path Sum
category: Leetcode
---

* content
{:toc}

## Minimum Path Sum

### 题目描述

> Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.
>
> Note: You can only move either down or right at any point in time.

### 解法

代码如下:
		
    public static int minPathSum( int[][] grid ) {

        int m = grid.length, n = grid[0].length;
        int[][] dp = new int[m][n];
        dp[0][0] = grid[0][0];
        for( int i = 1; i < m; ++i )
            dp[i][0] = grid[i][0]+dp[i-1][0];

        for( int j = 1; j < n; ++j )
            dp[0][j] = grid[0][j]+dp[0][j-1];

        for( int i = 1; i < m; ++i )
            for( int j = 1; j < n; ++j )
                dp[i][j] = Math.min( dp[i-1][j], dp[i][j-1] ) + grid[i][j];

        return dp[m-1][n-1];
    }
        
思考过程: `动态规划`. `当前格子`的最小值是由`左边一格`或`上边一格`加上当前的值组成的. 也就是

* `dp[i][j]`: 在第`[i][j]`个格子的最小值
* 最优子结构: `dp[i][j] = Math.min( dp[i-1][j], dp[i][j-1] ) + grid[i][j]`
* 重复子问题: `dp[i-1][j]`和`dp[i][j-1]`中有共同的子问题`dp[i-1][j-1]`

时空复杂度: 时间复杂度是O(n^2), 空间复杂度是O(n^2)