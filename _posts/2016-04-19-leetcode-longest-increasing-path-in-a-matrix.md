---
layout: post
title: Longest Increasing Path in a Matrix
category: Leetcode
---

* content
{:toc}

## Longest Increasing Path in a Matrix

### 题目描述

> Given an integer matrix, find the length of the longest increasing path.
>
> From each cell, you can either move to four directions: left, right, up or down. You may NOT move diagonally or move outside of the boundary (i.e. wrap-around is not allowed).
>
> Example 1:
>
    nums = [
      [9,9,4],
      [6,6,8],
      [2,1,1]
    ]
>
> Return `4`
> 
> The longest increasing path is `[1, 2, 6, 9]`.
>
> Example 2:
>
    nums = [
      [3,4,5],
      [3,2,6],
      [2,2,1]
    ]
>
> Return 4
> 
> The longest increasing path is `[3, 4, 5, 6]`. 
> 
> Moving diagonally is not allowed.

### 解法

代码如下:

    // 方向: 上, 左, 下, 右
    static int[][] dir = { {-1,0},{0,-1},{1,0},{0,1} };
    static int res = 0;

    public static int longestIncreasingPath(int[][] matrix) {
        if( matrix == null || matrix.length == 0 ) return 0;
        int n = matrix.length;
        int m = matrix[0].length;

        // 记忆数组: mem[i][j][k]表示(i,j)点在方向k上的最长递增路径长度
        int[][][] mem = new int[n][m][dir.length];
        for( int i = 0; i < n; ++i )
            for( int j = 0; j < m; ++j )
                Arrays.fill( mem[i][j], -1 );
        // 开始搜索
        for( int i = 0; i < n; ++i )
            for( int j = 0; j < m; ++j )
                for( int k = 0; k < dir.length; ++k ) {
                    mem[i][j][k] = dfs( matrix, i, j, k, mem ) + 1;
                    res = Math.max( res, mem[i][j][k] );
                }
        return res;
    }

    private static int dfs( int[][] matrix, int i, int j, int d, int[][][] mem ) {
        int n = matrix.length;
        int m = matrix[0].length;
        // (i, j)点在方向d上没有递增路径
        int ni = i + dir[d][0], nj = j + dir[d][1];
        if( ni < 0 || ni >= n ) return 0;
        if( nj < 0 || nj >= m ) return 0;
        if( matrix[ni][nj] <= matrix[i][j] ) return 0;
        // 搜索(i, j)节点在d方向上的最长递增路径长度
        int max = 0;
        for( int k = 0; k < dir.length; ++k ) {
            if( d == (k+2)%dir.length ) continue;
            if( mem[ni][nj][k] == -1 )
                mem[ni][nj][k] = dfs( matrix, ni, nj, k, mem ) + 1;
            max = Math.max( max, mem[ni][nj][k] );
        }
        return max;
    }

思考过程: `回溯法+动态规划`.

要查找最长递增路径的长度, 那么每一个节点都有可能是入口点, 对每一个入口点要考虑4个方向上的路径, 因此可以使用回溯法来遍历所有的解空间.

单纯使用回溯法会超时, 因此继续思考优化. 首先想到有没有可能子问题的解空间重叠了, 发现确实会出现子问题解空间重叠的情况, 例如:

* 求解`[i][j][down]`的时候需要`[ni][nj][x](x = { left, down, right })`
* 求解`[i+2][j][up]`的时候需要`[ni][nj][x](x = { left, up, right })`

也就是`(i, j)`在`down`方向上的解空间与`(i+2, j)`在`up`方向上的解空间重叠了.

因此借助记忆数组可以解决重复搜索解空间的时间消耗问题.

时空复杂度: 时间复杂度是`O(4*n^2)`, 空间复杂度是`O(4*n^2)`