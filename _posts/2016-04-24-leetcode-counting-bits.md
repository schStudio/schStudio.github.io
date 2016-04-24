---
layout: post
title: Counting Bits
category: Leetcode
---

* content
{:toc}

## Counting Bits

### 题目描述

> Given a non negative integer number num. For every numbers `i` in the range `0 ≤ i ≤ num` calculate the number of 1's in their binary representation and return them as an array.
> 
> Example:
> 
> For `num = 5` you should return `[0,1,1,2,1,2]`.
> 
> Follow up:
> 
> * It is very easy to come up with a solution with run time `O(n*sizeof(integer))`. But can you do it in linear time `O(n)` /possibly in a single pass?
> * Space complexity should be `O(n)`.
> * Can you do it like a boss? Do it without using any builtin function like `__builtin_popcount` in c++ or in any other language.


### 解法

代码如下:

    public static int[] countBits(int num) {
        int[] res = new int[num+1];
        for( int i = 1; i <= num; ++i )
            res[i] = res[i/2] + ( i & 1 );
        return res;
    }

思考过程: `动态规划`

一个元素的二进制表示左移一位, 会得到一个更大的值, 而且二进制表示`1`的个数是相等的.

也就是说, 一个比较大的值二进制表示`1`的个数可以由较小的值的二进制表示`1`的个数来计算.

这条规则也就满足最优子结构性质, 所以这道题用动态规划来解决.

当前值二进制表示`1`的个数, 等于当前值右移一位得到的值的二进制表示`1`的个数, 加上最低位1的个数.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(n)`