---
layout: post
title: Game of Life
category: Leetcode
---

* content
{:toc}

## Set Matrix Zeroes

### 题目描述

> According to the Wikipedia's article: "The Game of Life, also known simply as Life, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."
>
> Given a board with `m` by `n` cells, each cell has an initial state `live (1)` or `dead (0)`. Each cell interacts with its eight neighbors (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):
>
> * Any live cell with fewer than two live neighbors dies, as if caused by under-population.
> * Any live cell with two or three live neighbors lives on to the next generation.
> * Any live cell with more than three live neighbors dies, as if by over-population..
> * Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.
>
> Write a function to compute the next state (after one update) of the board given its current state.
>
> Follow up: 
>
> Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.
>
> In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?


### 解法

代码如下:

    public static void gameOfLife( int[][] board ) {
        if( board == null || board.length == 0 ) return;
        int m = board.length, n = board[0].length;
        for( int i = 0; i < m; ++i )
            for( int j = 0; j < n; ++j ) {
                int lives = livesOfNeighbors( board, m, n, i, j );
                if( (board[i][j] & 1) == 1 && lives >= 2 && lives <= 3 )
                    board[i][j] = 3;	// ( 0, 1 ) => ( 1, 1 )
                if( (board[i][j] & 1) == 0 && lives == 3 )
                    board[i][j] = 2;	// ( 0, 0 ) => ( 1, 0 )
            }
        for( int i = 0; i < m; ++i )
            for( int j = 0; j < n; ++j )
                board[i][j] >>= 1;
    }

    private static int livesOfNeighbors(int[][] board, int m, int n, int i, int j) {
        int lives = 0;
        for( int x = Math.max( i-1, 0 ); x <= Math.min( i+1, m-1 ); ++x )
            for( int y = Math.max( j-1, 0 ); y <= Math.min( j+1, n-1 ); ++y )
                lives += board[x][y] & 1;
        lives -= board[i][j] & 1;
        return lives;
    }

思考过程: 算法参考自[这里](https://leetcode.com/discuss/68352/easiest-java-solution-with-explanation)

因为不能使用新的空间, 所以需要在原数组空间进行记录.

已知一个细胞的状态只有`0`和`1`两种, 考虑使用`bit`作为单位进行记录.

细胞的当前状态可以是`0`和`1`两种, 记录在`最低位`, 现在使用`最低位前一位`作为细胞的下一次状态, 也即:

( 下一次状态, 当前状态 )

* `( dead, dead ) == ( 0, 0 )`
* `( dead, live ) == ( 0, 1 )`
* `( live, dead ) == ( 1, 0 )`
* `( live, live ) == ( 1, 1 )`

现在假设所有的细胞下一次状态都是`dead`, 然后接下来只需要考虑为`live`的状态即可.

根据题意可知, 细胞的下一次`live`状态变化如下:

* `live : board == 1 && lives == 2`
* `live : board == 1 && lives == 3`
* `live : board == 0 && lives == 3`

至此, 根据以上条件设置下一次状态, 然后使用右移操作更新状态, 把下一次状态更新到当前状态.

时空复杂度: 时间复杂度是`O(m*n)`, 空间复杂度是`O(1)`