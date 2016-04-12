---
layout: post
title: Sort Colors
category: Leetcode
---

* content
{:toc}

## Sort Colors

### 题目描述

> Given an array with n objects colored red, white or blue, sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.
>
> Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.
>
> Note:
>
> * You are not suppose to use the library's sort function for this 
>
> Follow up:
> A rather straight forward solution is a two-pass algorithm using counting sort.
>
> First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
>
> Could you come up with an one-pass algorithm using only constant space?

### 解法

代码如下:

    public static void sortColors( int[] nums ) {
        /*
         * low : [1...low-1]的元素全部为0
         * mid : [low...mid-1]的元素全部为1
         * high : [high+1...nums.length]的元素全部为2
         */
        int low = 0, mid = 0, high = nums.length-1;
        while( low <= mid && mid <= high ) {
            while( low <= mid && mid <= high && nums[low] == 0 ) { ++low; ++mid; }
            while( low <= mid && mid <= high && nums[high] == 2 ) --high;
            if( low > mid || mid > high ) break;
            if( nums[mid] == 0 ) swap( nums, low++, mid );
            else if( nums[mid] == 2 ) swap( nums, mid--, high-- );
            ++mid;
        }
    }

    public static void swap( int[] nums, int i, int j ) {
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }

思考过程: 处理过程类似快速排序中的一次`partition`, 但是不同地方在于扫描的方式:

* 快速排序 : 两边往中间扫描
* 该算法 : 从左边往右边扫描

只要理解了以下三个变量的意义, 也就明白了算法的处理过程:

* `low` : `low`维护了一个状态, 也就是`[1...low-1]`的值全部是0
* `mid` : `mid`维护了一个状态, 也就是`[low...mid-1]`的值全部是1, 同时`mid`作为处理指针
* `high` : `high`维护了一个状态, 也就是`[high+1...nums.length]`的值全部是2

处理过程如下 : 

* 如果`mid`指针指向的元素是0, 那么需要把它加入`low`的左边, 然后处理新元素; 
* 如果指向的元素是2, 那么需要把它加入`high`的右边, 然后处理新元素.

> 注意 :
>
> * 在元素2加入`high`的右边的时候, 需要保持处理指针`mid`原位置不变, 因为来自`high`指针的元素是未处理的新元素, 因为我们的算法是**从左往右扫描处理的**

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`
