---
layout: post
title: Count Primes
category: Leetcode
---

* content
{:toc}

## Count Primes

### 题目描述

> Count the number of prime numbers less than a non-negative number, `n`.

### 解法

代码如下:

    public static int countPrimes( int n ) {
        if( n < 2 ) return 0;
        boolean[] checked = new boolean[n];
        int count = 0;
        for( int i = 2; i < n; ++i ) {
            if( checked[i] ) continue;
            for( int j = 1; i * j < n; ++j )
                checked[i * j] = true;
            ++count;
        }
        return count;
    }

思考过程: 

直接筛选法: 对范围`[2...n-1]`内的每一个值`[i]`进行`isPrime`判断, `isPrime`的做法是遍历`[2...i-1]`判断是否有除数. 时间复杂度为`O(n^2)`.

优化`isPrime`(1) : 遍历范围变为`[2...i/2]`

优化`isPrime`(2) : 遍历范围变为`[2...Math.sqrt(i)]`

表格筛选法: 创建一个检查记录表格, 每次要检查值`[i]`之前, 先用检查记录表格查询是否检查过, 已经检查过的就直接跳过, 检查的值依次如下:

* `2`的倍数 : `2, 4, 6, 8, ...`
* `3`的倍数 : `3, 6, 9, 12, ...`
* `5`的倍数 : `5, 10, 15, 20, ...`

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(n)`