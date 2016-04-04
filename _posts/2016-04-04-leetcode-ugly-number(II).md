---
layout: post
title: Ugly Number(II)
category: Leetcode
---

* content
{:toc}

## Ugly Number

### 题目描述

> Write a program to check whether a given number is an ugly number.
>
> Ugly numbers are positive numbers whose prime factors only include 2, 3, 5. For example, 6, 8 are ugly while 14 is not ugly since it includes another prime factor 7.
>
> Note that 1 is typically treated as an ugly number.

### 解法

代码如下:
		
    public static boolean isUgly( int num ) {
        if( num == 0 ) return false;
        while( num % 2 == 0 )
            num /= 2;
        while( num % 3 == 0 )
            num /= 3;
        while( num % 5 == 0 )
            num /= 5;
        return num == 1;
    }
        
思考过程: `数学问题`. 一个数字`num`如果是`Ugly Number`, 那么这个`num`就一定能够被`2,3,5`整除, 所以只要一路整除下来, 最后判断结果就行了

时空复杂度: 时间复杂度是O(logn): n表示树的所有节点数, 空间复杂度是O(1)

- - -

## Ugly Number II

### 题目描述

> Write a program to find the n-th ugly number.
>
> Ugly numbers are positive numbers whose prime factors only include `2, 3, 5`. For example, `1, 2, 3, 4, 5, 6, 8, 9, 10, 12` is the sequence of the first `10` ugly numbers.
>
> Note that 1 is typically treated as an ugly number.


### 解法

代码如下:

    public static int nthUglyNumber( int n ) {

        int[] nums = new int[n];
        int index = 1;
        nums[0] = 1; 

        int count2 = 0, count3 = 0, count5 = 0;

        while( index < n ) {
            int num = Math.min(nums[count5]*5,
                    Math.min(nums[count2]*2, nums[count3]*3));
            nums[index++] = num;
            if( num % 2 == 0 ) count2++;
            if( num % 3 == 0 ) count3++;
            if( num % 5  == 0 ) count5++;
        }
        return nums[n-1];
    }

思考过程: 一个`Ugly Number`只能被`2,3,5`整除, 而且我们需要找出第`n`个`Ugly Number`, 那么我们就可以用`2,3,5`来动态构造`Ugly Number`.

我们使用`count2, count3, count5`分别表示`2,3,5使用的次数`. 每次构造`一个最小值`并且加入数组中(数组是排序的`Ugly Number`), 然后把用到的因数分别`+1`

时空复杂度: 时间复杂度是O(n), 空间复杂度是O(n)