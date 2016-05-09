---
layout: post
title: Rotate Image
category: Leetcode
---

* content
{:toc}

## Rotate Image

### 题目描述

> You are given an `n x n` 2D matrix representing an image.
> 
> Rotate the image by `90` degrees (clockwise).
> 
> Follow up:
> 
> Could you do this in-place?

### 解法

代码如下:

    public static void rotate(int[][] matrix) {
        int m = matrix.length;
        // 水平180°翻转
        for(int i = 0; i < m; i++)
            for(int j = 0; j < m / 2; j++)
                swap(matrix, i, j, i, m - 1 - j);
        // 对角线45°翻转
        for(int i = 0; i < m; i++)
            for(int j = m - 2 - i; j >= 0; j--)
                swap(matrix, i, j, m - 1 - j, m - 1 - i);
    }
    
    private static void swap(int[][] matrix, int i, int j, int x, int y) {
        int t = matrix[i][j];
        matrix[i][j] = matrix[x][y];
        matrix[x][y] = t;
    }

思考过程:

首先对矩阵水平180°翻转，然后再对角线45°翻转，就可以得到90°的旋转矩阵。

需要注意的地方就是矩阵中位置的关系，特别是对角线45°翻转，两个位置之间的关系满足：

* `i1 + j2 = m - 1`
* `j1 + i2 = m - 1`

找到这个对应关系是算法正确性的关键。

时空复杂度: 时间复杂度是`O(n^2)`, 空间复杂度是`O(1)`