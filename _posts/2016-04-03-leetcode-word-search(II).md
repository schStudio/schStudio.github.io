---
layout: post
title: Word Search(II)
category: Leetcode
---

* content
{:toc}

## Word Search

### 题目描述

> Given a 2D board and a word, find if the word exists in the grid.
>
> The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.
>
> For example,
> 
> Given board =
>
     [
       ['A','B','C','E'],
       ['S','F','C','S'],
       ['A','D','E','E']
     ]
>
> word = `"ABCCED"`, -> returns `true`,
>
> word = `"SEE"`, -> returns `true`,
> 
> word = `"ABCB"`, -> returns `false`.

### 解法

代码如下:
		
    // 方向: 上-左-下-右
    private static final int[][] dir = { {-1,0},{0,-1},{1,0},{0,1} };

    public static boolean exists( char[][] board, String word ) {
        // 特殊情况单独处理
        if( board == null || board.length == 0) {
            return false;
        }
        if( "".equals( word ) ) {
            return true;
        }

        int m = board.length;
        int n = board[0].length;
        // 路径记录表
        boolean[][] used = new boolean[m][n];

        for( int i = 0; i < m; ++i ) {
            for( int j = 0; j < n; ++j ) {
                if( board[i][j] == word.charAt(0) ) {
                    used[i][j] = true;
                    if( existsHelper( board, word, i, j, 1, used ) ) 
                        return true;
                    used[i][j] = false;
                }
            }
        }

        return false;
    }

    private static boolean existsHelper( char[][] board, String word, 
            int i, int j, int t, boolean[][] used ) {
        if( t == word.length() ) {
            return true;
        }
        for( int k = 0; k < dir.length; ++k ) {
            int iNext = i+dir[k][0];
            int jNext = j+dir[k][1];
            if( iNext >= 0 && iNext < board.length &&
                 jNext >= 0 && jNext < board[0].length &&
                 used[iNext][jNext] == false &&
                 board[iNext][jNext] == word.charAt(t) ) {
                // 进入下一格
                used[iNext][jNext] = true;
                if( existsHelper( board, word, iNext, jNext, t+1, used ) ) {
                    return true;
                }
                used[iNext][jNext] = false;
            }
        }
        return false;
    }
        
思考过程: `回溯法`+`DFS`. 首先找到一个入口点, 之后一路进行`深度优先搜索`.

时空复杂度: 时间复杂度是O(n^3), 空间复杂度是O(n^2)