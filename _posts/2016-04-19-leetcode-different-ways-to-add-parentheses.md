---
layout: post
title: Different Ways to Add Parentheses
category: Leetcode
---

* content
{:toc}

## Different Ways to Add Parentheses

### 题目描述

> Given a string of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. The valid operators are `+`, `-` and `*`.
>
>
> Example 1
> Input: `"2-1-1"`.
>
    ((2-1)-1) = 0
    (2-(1-1)) = 2
>
> Output: `[0, 2]`
>
> Example 2
> Input: `"2*3-4*5"`
>
    (2*(3-(4*5))) = -34
    ((2*3)-(4*5)) = -14
    ((2*(3-4))*5) = -10
    (2*((3-4)*5)) = -10
    (((2*3)-4)*5) = 10
>
> Output: `[-34, -14, -10, -10, 10]`

### 解法

代码如下:

    public static List<Integer> diffWaysToCompute( String input ) {
        List<Integer>[][] dp = new List[input.length()][input.length()];
        return  helper( dp, input, 0, input.length()-1 );
    }
    private static List<Integer> helper( List<Integer>[][] dp, 
            String input, int low, int high ) {
        if( dp[low][high] != null ) return dp[low][high];
        List<Integer> res = new ArrayList<>();
        for( int i = low; i <= high; ++i ) {
            char ch = input.charAt( i );
            if( ch == '-' || ch == '+' || ch == '*' ) {
                List<Integer> part1 = helper( dp, input, low, i-1 );
                List<Integer> part2 = helper( dp, input, i+1, high );
                for( int val1 : part1 )
                    for( int val2 : part2 )
                        switch( ch ) {
                        case '-' : res.add( val1 - val2 ); break;
                        case '+' : res.add( val1 + val2 ); break;
                        case '*' : res.add( val1 * val2 ); break;
                        }
            }
        }
        if( res.size() == 0 )
            res.add( Integer.parseInt( input.substring( low, high+1 ) ) );
        return res;
    }

思考过程: `分治法+动态规划`. 算法参考自[这里](https://leetcode.com/discuss/48477/a-recursive-java-solution-284-ms).

一个表达式可以表示为`exp1 [-|+|*] exp2`的形式, 所以这道题具有子问题性质, 可以用分治法来解决.

首先遍历所有的操作符, 对左边表达式求结果, 对右边表达式求结果, 然后通过操作符组合两个表达式, 就得到了最终结果.

但是有个问题: 从左到右遍历操作符并求子表达式的时候, 包含了很多重复情况, 因此借助动态规划的记忆数组来保存求过的子表达式, 加快运算.

> 通过比较, 没有使用记忆数组的结果为`9ms(27.25%)`, 而使用记忆数组的结果为`2ms(99.86%)`

时空复杂度: 时间复杂度是`O(n^2)`, 空间复杂度是`O(n^2)`