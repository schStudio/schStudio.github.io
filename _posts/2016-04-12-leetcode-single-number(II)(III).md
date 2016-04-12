---
layout: post
title: Single Number(II)(III)
category: Leetcode
---

* content
{:toc}

## Single Number

### 题目描述

> Given an array of integers, every element appears twice except for one. Find that single one.
>
> Note:
>
> * Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

### 解法

代码如下:

    public static int singleNumber( int[] nums ) {
        int res = 0;
        for( int i = 0; i < nums.length; ++i )
            res ^= nums[i];
        return res;
    }

思考过程: 借助异或操作的特性:

* A<sub>i</sub> ^ A<sub>i</sub> == 0
* 异或运算有交换性

假设A<sub>k</sub>就是那个`Single Number`, 那么以下式子是成立的:

* A<sub>k</sub> = (A<sub>1</sub> ^ A<sub>1</sub>) ^ (A<sub>2</sub> ^ A<sub>2</sub>) ... (A<sub>k</sub>) ... (A<sub>n</sub> ^ A<sub>n</sub>)

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`

- - -

## Single Number II

### 题目描述

> Given an array of integers, every element appears three times except for one. Find that single one.
>
> Note:
>
> * Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

### 解法

代码如下:

    public static int singleNumber( int[] nums ) {
        int low = 0, high = 0;
        for( int i = 0; i < nums.length; ++i ) {
            low = ( low ^ nums[i] ) & ~high;
            high = ( high ^ nums[i] ) & ~low;
        }
        return low;
    }

思考过程: 直接看代码很难理解, 该算法参考自[这里](https://leetcode.com/discuss/6632/challenge-me-thx), 下面简单介绍下算法思想.

我们用一个状态来表示数字出现的个数, 因为数组中的元素最多3个, 所以状态可以用`00`, `01`, `10`这三种来表示. 接下来就可以得出状态的转移: `00` -> `01` -> `10` -> `00`.

根据这个思想, 我们定义两个变量`low`表示低位和`high`表示高位, 然后可以定义出它们的状态转移过程:

* `low` : `0` -> `1` -> `0` -> `0`, 最后一个转移当`high`为`1`的时候才发生
* `high` : `0` -> `0` -> `1` -> `0`, 第一个转移当`low*`为`1`的时候才发生

> `low*`表示当前的`low`, 而不是前一个`low`, 所以代码中的`high`要放在`low`的后面

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`

- - -

## Single Number III

### 题目描述

> Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.
>
> For example:
>
> Given `nums = [1, 2, 1, 3, 2, 5]`, return `[3, 5]`.
>
> Note:
>
> * The order of the result is not important. So in the above example, [5, 3] is also correct.
> * Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?


### 解法

代码如下:

    public static int[] singleNumber( int[] nums ) {

        int tmp = 0;
        for( int i = 0; i < nums.length; ++i )
            tmp ^= nums[i];

        // 保留二进制表示中最后一位1
        tmp = tmp & (~tmp+1);

        int res1 = 0, res2 = 0;
        for( int i = 0; i < nums.length; ++i ) {
            if( (nums[i] & tmp) != 0 )
                res1 ^= nums[i];
            else
                res2 ^= nums[i];
        }
        return new int[]{ res1, res2 };
    }

思考过程: 算法思想与`Single Number`差不多, 但是处理过后的两个`Single Number`会揉合在一起, 我们需要进一步操作把这两个值区分开来. 区分方法如下:

首先找到区分标准, 而揉合结果`tmp`二进制表示中的最后一位1就是区分标准, 因为`1 = 0 ^ 1`, 所以找到区分标准, 再遍历一遍数组, 但是这次要根据区分标准来划分进行异或运算.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`