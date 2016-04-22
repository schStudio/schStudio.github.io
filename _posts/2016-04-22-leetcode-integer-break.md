---
layout: post
title: Integer Break
category: Leetcode
---

* content
{:toc}

## Integer Break

### 题目描述

> Given a positive integer n, break it into the sum of at least two positive integers and maximize the product of those integers. Return the maximum product you can get.
>
> For example, 
> 
> given `n = 2`, return `1 (2 = 1 + 1)`;
>
> given `n = 10`, return `36 (10 = 3 + 3 + 4)`.
>
> Note: you may assume that n is not less than `2`.

### 解法

代码如下:

    public static int integerBreak( int n ) {
        if( n == 2 ) return 1;
        if( n == 3 ) return 2;
        int count3 = n / 3;
        int remain = n % 3;
        switch( remain ) {
        case 0 : return (int)Math.pow( 3, count3 );
        case 1 : return (int)Math.pow( 3, count3-1 ) * 4;
        case 2 : return (int)Math.pow( 3, count3 ) * 2;
        }
        return -1;
    }

思考过程: 

对于这种比较难以下手的问题, 一开始先从简单的例子开始:

* `2` : `1*1=1`
* `3` : `1*2=2`
* `4` : `2*2=4`
* `5` : `2*3=6`
* `6` : `3*3=9`

可以发现`2-6`的结果都是拆成`{1,2,3}`这3个数字, 对于`6`之后的数字, 也必然会拆成这3个数字, 因为后面的数字都可以由前面的数字组成, 而前面的数字又必然拆成这3个数字. 

例如数字`7`必然由`1-6`组成, 所以数字`7`最终结果也会拆成`{1,2,3}`这3个数字, 结果是`3*2*2`.

例如数字`8`必然由`1-7`组成, 所以数字`8`最终结果也会拆成`{1,2,3}`这3个数字, 结果是`3*3*2`.

...

然后继续思考发现`3*3 > 2*2*2`, 也就是说我们应该尽量拆成3, 

但是`3*1 < 2*2`, 所以如果不断拆分之后产生数字`1`, 应该取出一个`3`进行补回, 然后拆成`2*2`.

时空复杂度: 时间复杂度是`O(Math.pow)`, 空间复杂度是`O(1)`