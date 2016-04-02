---
layout: post
title: UniquePaths(II)
category: Leetcode
---

* content
{:toc}

## UniquePaths

### 题目描述

> A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).
>
> The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).
>
> How many possible unique paths are there?
>
> Note: m and n will be at most 100.
>
> ![unique-path](http://articles.leetcode.com/wp-content/uploads/2014/12/robot_maze.png)

### 解法

代码如下:
		
    public static int uniquePaths( int m, int n ) {
        // dp[i][j]: 在[i][j]格子的所有可能Path
        int[][] dp = new int[m][n];
        // 起点位置, 竖线Path为1
        for( int i = 0; i < m; ++i )
            dp[i][0] = 1;
        // 起点位置, 横线Path为1
        for( int j = 0; j < n; ++j )
            dp[0][j] = 1;

        for( int i = 1; i < m; ++i )
            for( int j = 1; j < n; ++j )
                dp[i][j] = dp[i-1][j] + dp[i][j-1];

        return dp[m-1][n-1];
    }
        
思考过程: `动态规划`. `当前格子`可以由`左边格子`和`上边格子`的路径数目相加, 所以定义

* `dp[i][j]`: 在第`[i][j]`个格子的所有可能路径数
* 最优子结构: `dp[i][j]` = `dp[i-1][j]` + `dp[i][j-1]`
* 重叠子问题: `dp[i-1][j]`和`dp[i][j-1]`都需要子问题`dp[i-1][j-1]`的值

时空复杂度: 时间复杂度是O(mn), 空间复杂度是O(mn)

- - -

## UniquePaths II

### 题目描述

> Follow up for "Unique Paths":
>
> Now consider if some obstacles are added to the grids. How many unique paths would there be?
>
> An obstacle and empty space is marked as 1 and 0 respectively in the grid.
>
> For example,
> There is one obstacle in the middle of a 3x3 grid as illustrated below.
>
> 	[
> 		[0,0,0],
>   	[0,1,0],
>   	[0,0,0]
> 	]
> 
> The total number of unique paths is 2.
>
> Note: `m` and `n` will be at most 100.

### 解法

代码如下:
		
    public static int uniquePaths( int[][] obstacleGrid ) {
        if( obstacleGrid.length == 0 ) return 0;

        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        // dp[i][j]: 在[i][j]格子的所有可能Path
        int[][] dp = new int[m][n];
        // 起点位置, 竖线Path为1, 遇到障碍之后的为0
        int i = 0;
        for( ; i < m && obstacleGrid[i][0]==0; ++i )
            dp[i][0] = 1;
        for( ; i < m; ++i )
            dp[i][0] = 0;
        // 起点位置, 横线Path为1, 遇到障碍之后的为0
        int j = 0;
        for( ; j < n && obstacleGrid[0][j]==0; ++j )
            dp[0][j] = 1;
        for( ; j < n; ++j )
            dp[0][j] = 0;

        for( i = 1; i < m; ++i )
            for( j = 1; j < n; ++j )
                if( obstacleGrid[i][j] == 0 )
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
                else
                    dp[i][j] = 0;

        return dp[m-1][n-1];
    }
	
思考过程: 同UniquePaths相同, 在此基础上多了障碍的设置, 那么我们只需要考虑障碍带来的影响就可以了: 

1. 初始化横线和竖线的时候, 如果遇到障碍, 之后都是不可达的
2. 在递推过程中, 如果遇到障碍, 那么该格子是不可达的, 值为0

时空复杂度: 时间复杂度是(mn), 空间复杂度是(mn)