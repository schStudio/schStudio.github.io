---
layout: post
title: H-Index(II)
category: Leetcode
---

* content
{:toc}

## H-Index

### 题目描述

> Given an array of citations (each citation is a non-negative integer) of a researcher, write a function to compute the researcher's h-index.
> 
> According to the definition of [h-index on Wikipedia](https://en.wikipedia.org/wiki/H-index): "A scientist has index `h` if `h` of his/her `N` papers have **at least** `h` citations each, and the other `N − h` papers have **no more than** `h` citations each."
> 
> For example, given `citations = [3, 0, 6, 1, 5]`, which means the researcher has `5` papers in total and each of them had received `3, 0, 6, 1, 5` citations respectively. Since the researcher has `3` papers with at least `3` citations each and the remaining two with no more than `3` citations each, his h-index is `3`.
> 
> Note: If there are several possible values for `h`, the maximum one is taken as the h-index.


### 解法

代码如下:

    public static int hIndex( int[] citations ) {
        Arrays.sort( citations );
        for( int i = 0; i < citations.length; ++i )
            if( citations[i] >= citations.length - i )
                return citations.length - i;
        return 0;
    }

思考过程: 

根据题意, 知道`h-index`与元素个数和数值大小有关. 所以可以先排序.

元素的个数可以根据数组下标确定, 数值大小根据元素值确定.

如果当前元素值大于等于元素的个数, 说明后面的元素也都大于等于元素的个数, 这就说明找到`h-index`

时空复杂度: 时间复杂度是`O(nlogn)`, 空间复杂度是`O(1)`

- - -

## H-Index II

### 题目描述

> Follow up for H-Index: What if the citations array is sorted in ascending order? Could you optimize your algorithm?
> 
> Hint:
> 
> Expected runtime complexity is in `O(logn)` and the input is sorted.

### 解法

代码如下:

    public static int hIndex( int[] citations ) {
        int low = 0, high = citations.length-1;
        while( low <= high ) {
            int mid = low + ( high - low ) / 2;
            if( citations.length - mid <= citations[mid] )
                high = mid - 1;
            else
                low = mid + 1;
        }
        return citations.length - low;
    }

思考过程: `二分搜索`.跟上面一样的思考方式.

每次找到范围内的中间值, 如果中间值大于等于元素个数, 为了获得更大的h-index, 向左边继续搜索.

时空复杂度: 时间复杂度是`O(logn)`, 空间复杂度是`O(1)`