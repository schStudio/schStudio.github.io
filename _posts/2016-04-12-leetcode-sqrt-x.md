---
layout: post
title: Sqrt(x)
category: Leetcode
---

* content
{:toc}

## Sqrt(x)

### 题目描述

> Implement int `sqrt(int x)`.
>
> Compute and return the square root of `x`.

### 解法

代码如下:

    public static int mySqrt( int x ) {
        int low = 0, high = 0x00010000;
        while( low <= high ) {
            int mid = low + (high-low)/2;
            int midPow2 = mid * mid;
            if( midPow2 == x )
                return mid;
            else if( midPow2 > 0 && midPow2 < x )
                low = mid + 1;
            else
                high = mid - 1;
        }
        return high;
    }

思考过程: `二分搜索`, 首先定义搜索范围, 因为`int`类型是32位的, 所以搜索范围在`[0...2^16]`, 之后在这个范围进行二分搜索.

> 注意: 
> 
> * 二分搜索过程中一定要判断越界问题
> * 边界问题, 结果是向下取整的, 应该返回`high`

时空复杂度: 时间复杂度是`O(logx)`, 空间复杂度是`O(1)`
