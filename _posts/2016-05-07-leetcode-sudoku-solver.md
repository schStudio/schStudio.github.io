---
layout: post
title: Sudoku Solver
category: Leetcode
---

* content
{:toc}

## Sudoku Solver

### 题目描述

> Write a program to solve a Sudoku puzzle by filling the empty cells.
> 
> Empty cells are indicated by the character `'.'`.
> 
> You may assume that there will be only one unique solution.
> 
> ![](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)
> 
> A sudoku puzzle...
> 
> ![](https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png)
> 
> ...and its solution numbers marked in red.

### 解法

代码如下:

    private static int[] hor = new int[9];
    private static int[] ver = new int[9];
    private static int[][] grid = new int[3][3];
    
    public static void solveSudoku(char[][] board) {
        int m = board.length, n = board[0].length;
        for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++)
                if(board[i][j] != '.')
                    set(i, j, board[i][j] - '0');
        int[] next = next(board, 0, -1);
        fill(board, next[0], next[1]);
    }
    
    private static boolean fill(char[][] board, int x, int y) {
        int m = board.length, n = board[0].length;
        if(x == m && y == n) return true;
        for(int i = 1; i <= 9; ++i) {
            if(check(x, y, i)) {
                board[x][y] = (char) (i + '0');
                set(x, y, i);
                int[] next = next(board, x, y);
                if(fill(board, next[0], next[1]))
                    return true;
                board[x][y] = '.';
                unset(x, y, i);
            }
        }
        return false;
    }
    
    private static int[] next(char[][] board, int x, int y) {
        int m = board.length, n = board[0].length;
        while(x < m) {
            while(++y < n)
                if(board[x][y] == '.') return new int[]{x, y};
            x++;
            y = -1;
        }
        return new int[]{m, n};
    }

    private static boolean check(int m, int n, int x) {
        if((hor[m] & (1 << x)) != 0)
            return false;
        if((ver[n] & (1 << x)) != 0)
            return false;
        if((grid[m/3][n/3] & (1 << x)) != 0)
            return false;
        return true;
    }
    private static void set(int m, int n, int x) {
        hor[m] |= (1 << x);
        ver[n] |= (1 << x);
        grid[m/3][n/3] |= (1 << x);
    }
    private static void unset(int m, int n, int x) {
        hor[m] ^= (1 << x);
        ver[n] ^= (1 << x);
        grid[m/3][n/3] ^= (1 << x);
    }

思考过程: `哈希表+回溯法`。

求出数独的一个解，可以使用回溯法来暴力查找，过程如下：

1. 找到一个空白的位置，遍历尝试填写数字`1-9`
2. 如果填写的数字在**行**，**列**，**子方格**中已经出现过，那么尝试下一个数字
3. 如果填写的数字在**行**，**列**，**子方格**中都没有出现过，那么填写，然后进入步骤1

以上就是回溯法的暴力过称，可以发现我们在查询数字是否出现在**行**，**列**，**子方格**的时候，是用遍历比较的方式来做的，这样效率是十分低下的。

因此，如果借助哈希表来记录**行**，**列**，**子方格**的填写情况，那么就可以优化这个查询时间，所以代码中的几个成员变量的含义如下：

* `hor[i]` : 第`i`行方向的数字填写记录
* `ver[i]` : 第`i`列方向上的数字填写记录
* `grid[i][j]` : 子方格`i`行`j`列的数字填写记录

需要注意的地方是，这里的哈希表是借助了位运算来实现的：

一个int类型变量相当于一个哈希表，从低位往高位方向，第`1-9`位对应着数字`1-9`。

所以`check`, `set`, `unset`都是借助位运算来实现的。

因此，算法流程就变为：

1. 遍历棋盘的初始状态，记录**行**，**列**，**子方格**的数字填写记录
2. 找到一个空白的位置，遍历尝试填写数字`1-9`
3. 借助`hor[]`, `ver[]`, `grid[][]`查询数字是否已经填写
4. 如果已经填写，尝试下一个数字
5. 如果没有填写，填写棋盘并填写数字记录，进入步骤2

时空复杂度: 时间复杂度是`O(n^2)`, 空间复杂度是`O(1)`