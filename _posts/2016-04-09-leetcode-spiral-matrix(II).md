---
layout: post
title: Spiral Matrix(II)
category: Leetcode
---

* content
{:toc}

## Spiral Matrix

### 题目描述

> Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.
>
> For example,
> 
> Given the following matrix:
>
    [
      [ 1, 2, 3 ],
      [ 4, 5, 6 ],
      [ 7, 8, 9 ]
    ]
>
> You should return `[1,2,3,6,9,8,7,4,5]`.

### 解法

代码如下:

    public List<Integer> spiralOrder( int[][] matrix ) {

        List<Integer> res = new ArrayList<>();
        if( matrix.length == 0 ) return res;

        int upBound = 0, downBound = matrix.length-1;
        int leftBound = 0, rightBound = matrix[0].length-1;

        while( upBound <= downBound && leftBound <= rightBound ) {
            // 左 -> 右
            for( int i = leftBound; i <= rightBound; ++i ) 
                res.add( matrix[upBound][i] );
            ++upBound;

            // 上 -> 下
            for( int i = upBound; i <= downBound; ++i )
                res.add( matrix[i][rightBound] );
            --rightBound;

            // 右 -> 左
            if( upBound <= downBound )
                for( int i = rightBound; i >= leftBound; --i )
                    res.add( matrix[downBound][i] );
            --downBound;

            // 下 -> 上
            if( leftBound <= rightBound )
                for( int i = downBound; i >= upBound; --i )
                    res.add( matrix[i][leftBound] );
            ++leftBound;
        }

        return res;
    }


思考过程: 首先定义四个边界, 分别是上右下左, 只要四个边界不出现越界(`上边界<=下边界 && 左边界<=右边界`), 说明还没有扫描完.

一次扫描表示四个边界分别遍历一次, 但是要注意`右->左`和`下->上`这两个遍历需要先判断会不会分别和`左->右`和`上->下`重复遍历.

时空复杂度: 时间复杂度为`O(n^2)`, 空间复杂度为`O(1)`

- - -

## Spiral Matrix II

### 题目描述

> Given an integer n, generate a square matrix filled with elements from `1 to n2` in spiral order.
>
> For example,
> Given `n = 3`,
>
> You should return the following matrix:
>
    [
     [ 1, 2, 3 ],
     [ 8, 9, 4 ],
     [ 7, 6, 5 ]
    ]

### 解法

代码如下:

    public int[][] generateMatrix( int n ) {

        int[][] matrix = new int[n][n];

        int upBound = 0, downBound = n-1;
        int leftBound = 0, rightBound = n-1;

        int ct = 1;
        while( upBound <= downBound && leftBound <= rightBound ) {
            // 左 -> 右
            for( int i = leftBound; i <= rightBound; ++i ) 
                matrix[upBound][i] = ct++;
            ++upBound;

            // 上 -> 下
            for( int i = upBound; i <= downBound; ++i )
                matrix[i][rightBound] = ct++;
            --rightBound;

            // 右 -> 左
            if( upBound <= downBound )
                for( int i = rightBound; i >= leftBound; --i )
                    matrix[downBound][i] = ct++;
            --downBound;

            // 下 -> 上
            if( leftBound <= rightBound )
                for( int i = downBound; i >= upBound; --i )
                    matrix[i][leftBound] = ct++;
            ++leftBound;
        }

        return matrix;
    }

思考过程: 算法思想跟上一题一模一样, 稍微修改就可以了

时空复杂度: 时间复杂度为`O(n^2)`, 空间复杂度为`O(n^2)`