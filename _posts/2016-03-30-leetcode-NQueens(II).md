---
layout: post
title: NQueens(II)
category: Leetcode
---

* content
{:toc}

## NQueens

### 题目描述

> The n-queens puzzle is the problem of placing n queens on an n×n chessboard such > that no two queens attack each other.
>
> ![8-Queens](http://articles.leetcode.com/wp-content/uploads/2012/03/8-queens.png)
>
> Given an integer n, return all distinct solutions to the n-queens puzzle.
> 
> Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space respectively.
> For example,
> There exist two distinct solutions to the 4-queens puzzle:

	[
     [".Q..",  // Solution 1
      "...Q",
      "Q...",
      "..Q."],

     ["..Q.",  // Solution 2
      "Q...",
      "...Q",
      ".Q.."]
    ]

### 解法

* 递归方式

    代码如下:

        public static List<List<String>> solveNQueensRecursion( int n ) {
            List<List<String>> res = new ArrayList<>();
            // 创建棋盘: board[i]表示第i行第board[i]列
            int[] board = new int[n];
            // 初始化棋盘, -1表示没有棋子
            Arrays.fill(board, -1);
            // 回溯查找解
            helperNQueens( 0, board, res );
            return res;
        }

        private static void helperNQueens(int t, int[] board, List<List<String>> res) {
            if( t == board.length ) {
                res.add( newBoard(board) );
                return;
            }
            for( int i = 0; i < board.length; ++i ) {
                board[t] = i;
                if( constraints( board, t ) )
                    helperNQueens( t+1, board, res );
            }
        }

        private static boolean constraints(int[] board, int t) {
            for( int i = 0; i < t; ++i ) {
                if( (board[i] == board[t]) ||
                        (t-i == board[t]-board[i] || t-i == board[i]-board[t]) )
                    return false;
            }
            return true;
        }

        /*
         * 返回棋盘的表示形式, 类型为List<String>
         */
        private static List<String> newBoard(int[] board) {
            List<String> res = new ArrayList<>();
            int n = board.length;
            char[][] newBoard = new char[n][n];
            for( int i = 0; i < n; ++i )
                Arrays.fill(newBoard[i], '.');
            for( int i = 0; i < n; ++i ) {
                newBoard[i][board[i]] = 'Q';
                StringBuilder sb = new StringBuilder();
                for( char c : newBoard[i] ) sb.append(c);
                res.add( sb.toString() );
            }
            return res;
        }

思考过程: N皇后问题属于经典的题目, 用`回溯法`来解决

> 疑难点: 难点主要有两个, 一个是`棋盘的表示`, 另一个是皇后之间的`冲突判断`
>
> * `棋盘的表示`要建立在对问题约束条件的理解之上. 棋盘之中皇后之间不能处于同一行, 也不能处于同一列, 那么我们可以用一个数组board来表示这个棋盘. 
> 
> 	* 数组下标表示棋盘中的行值
> 	* 数组的值表示棋盘中的列值
> 	
> * `冲突判断`有3个条件: 
> 
> 	* 皇后不能处于同一行: 不需要显示判断, 因为我们以行作为递归, 皇后不可能出现在同一行
> 	* 皇后不能处于同一列: 根据 board[i] == board[j] 作为判断
> 	* 皇后不能处于同一斜线: 根据 j-i == abs(board[j]-board[i]) (j>i)
>
> 斜线的判断可能比较难以理解: 意思是行值差与列值差相等表示皇后在同一斜线上.

时空复杂度: 时间复杂度是O(n!), 空间复杂度是O(n): 递归的空间, 排除构造出来的棋盘

### 总结

> 善于观察题目的限制条件, 从而构造出合适的数据结构

* 迭代方式

    代码如下:

        public static List<List<String>> solveNQueensIteration( int n ) {
            List<List<String>> res = new ArrayList<>();
            // 创建棋盘: board[i]表示第i行第board[i]列
            int[] board = new int[n];

            // 初始化第0行的状态
            board[0] = -1;
            // t表示目前处于第t行
            int t = 0;
            while( t >= 0 ) {
                // 查找第t行皇后的存放位置
                ++board[t];
                while( board[t] < n && !constraints( board, t ) ) ++board[t];

                // 找到第t行皇后的存放位置就进入下一行
                if( board[t] < n ) {
                    if( t == n-1 ) {
                        res.add( newBoard(board) );
                    } else {
                        ++t;
                        board[t] = -1;
                    }
                } else {
                    --t;
                }
            }
            return res;
        }

- - -

## NQueens II

### 题目描述

> Follow up for N-Queens problem.
>
> Now, instead outputting board configurations, return the total number of distinct solutions.
> ![8-Queens](http://articles.leetcode.com/wp-content/uploads/2012/03/8-queens.png)
>

### 解法

该题目的算法与上一道题的一模一样, 只需要在相应的`递归版本`或`迭代版本`中找到可行解的时候统计数量即可