---
layout: post
title: Triangle
category: Leetcode
---

* content
{:toc}

## Triangle

### 题目描述

> Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.
>
> For example, given the following triangle
> 
     [
          [2],
         [3,4],
        [6,5,7],
       [4,1,8,3]
     ]
>
> The minimum path sum from top to bottom is `11` (i.e., `2 + 3 + 5 + 1 = 11`).
>
> Note:
> 
> Bonus point if you are able to do this using only O(n) extra space, where n is the total number of rows in the triangle.

### 解法

代码如下:

    public static int minimumTotal( List<List<Integer>> triangle ) {
        for( int i = 0; i+1 < triangle.size(); ++i ) {
            List<Integer> row = triangle.get( i );
            List<Integer> nextRow = triangle.get( i+1 );
            for( int j = 0; j < nextRow.size(); ++j ) {
                if( j-1 >= 0 &&  j != nextRow.size()-1 ) {
                    nextRow.set( j, nextRow.get(j)+Math.min( row.get( j-1 ), row.get( j ) ) );
                } else if( j == nextRow.size()-1 ){
                    nextRow.set( j, nextRow.get(j)+row.get( j-1 ) );
                } else {
                    nextRow.set( j, nextRow.get(j)+row.get( j ) );
                }
            }
        }
        return Collections.min( triangle.get( triangle.size()-1 ) );
    }

思考过程: `动态规划`.

找出由顶向底的一条最小路径, 底部每一条路径都有可能是最小路径. 想要**求出最后一行每条路径的最小值**, 我们**需要先求出上一行每条路径的最小值**, 所以这道题具有最优子结构性质.

下一行的结果只需要当前行的结果就可以求出来, 而且我们只需要最小值, 所以可以在原数组空间进行递推演算.

时空复杂度: 时间复杂度是`O(n)`: `n`表示数字个数, 空间复杂度是`O(1)`
