---
layout: post
title: Set Matrix Zeroes
category: Leetcode
---

* content
{:toc}

## Set Matrix Zeroes

### 题目描述

> Given a `m x n` matrix, if an element is `0`, set its entire row and column to `0`. Do it in place.
>
> Follow up:
>
> Did you use extra space?
>
> A straight forward solution using `O(mn)` space is probably a bad idea.
>
> A simple improvement uses `O(m + n)` space, but still not the best solution.
>
> Could you devise a constant space solution?

### 解法

代码如下:

    public static void setZeros( int[][] matrix ) {
        boolean row0 = false, col0 = false;
        int m = matrix.length, n = matrix[0].length;
        // 搜索第0列是否为0
        for( int i = 0; i < m; ++i )
            if( matrix[i][0] == 0 ) {
                col0 = true;
                break;
            }
        // 搜索第0行是否为0
        for( int j = 0; j < n; ++j )
            if( matrix[0][j] == 0 ) {
                row0 =  true;
                break;
            }
        // 把结果压缩到第0列第0行
        for( int i = 1; i < m; ++i )
            for( int j = 1; j < n; ++j )
                if( matrix[i][j] == 0 ) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
        // 开始清0
        for( int i = 1; i < m; ++i )
            for( int j = 1; j < n; ++j )
                if( matrix[i][0] == 0 || matrix[0][j] == 0 )
                    matrix[i][j] = 0;
        // 如果第0列为0 : 第0列清0
        if( col0 )
            for( int i = 0; i < m; ++i )
                matrix[i][0] = 0;
        // 如果第0行为0 : 第0行清0
        if( row0 )
            for( int j = 0; j < n; ++j )
                matrix[0][j] = 0;
    }

思考过程: 

对于`matrix[i][j] == 0`, 其结果会把第`i`行和第`j`列都清0, 那么可以进行预处理, 先把结果保存到`matrix[i][0]`和`matrix[0][j]`, 之后根据第0行和第0列就可以开始清0的处理.

但是由于预处理过程中第0行和第0列会改变, 所以需要先用两个变量`row0`和`col0`来保存, 表示第0行和第0列是否需要清0, 最后进行清0的处理就可以了.

时空复杂度: 时间复杂度是`O(m*n)`, 空间复杂度是`O(1)`