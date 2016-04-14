---
layout: post
title: Basic Calculator(II)
category: Leetcode
---

* content
{:toc}

## Basic Calculator

### 题目描述

> Implement a basic calculator to evaluate a simple expression string.
>
> The expression string may contain open ( and closing parentheses ), the plus + or minus sign -, non-negative integers and empty spaces .
>
> You may assume that the given expression is always valid.
>
> Some examples:
>
> `"1 + 1" = 2`
>
> `" 2-1 + 2 " = 3`
>
> `"(1+(4+5+2)-3)+(6+8)" = 23`
>
> Note: Do not use the eval built-in library function.

### 解法

代码如下:

    public static int calculate( String s ) {
        if( s == null ) return 0;

        int res = 0;
        int sign = 1;
        int num = 0;

        Stack<Integer> stack = new Stack<>();
        stack.push( sign );

        for( int i = 0; i < s.length(); ++i ) {
            char ch = s.charAt( i );
            if( Character.isDigit(ch) ) {
                num = num * 10 + ch - '0';
            } else if( ch == '+' || ch == '-' ) {
                res += num * sign;
                sign = stack.peek() * ( ch == '+' ? 1 : -1 );
                num = 0;
            } else if( ch == '(' ) {
                stack.push( sign );
            } else if( ch == ')' ) {
                stack.pop();
            }
        }
        res += num * sign;
        return res;
    }

思考过程: 算法来自[这里](https://leetcode.com/discuss/39553/iterative-java-solution-with-stack). 以字符作为考虑基准, 有5种情况:

* `数字` : 继续求值(`num = num * 10 + 数字`)
* `+` : 计算前面表达式的结果, 计算完成后清除`num`
* `-` : 计算前面表达式的结果, 计算完成后清除`num`
* `(` : 把符号放进栈
* `)` : 把符号弹出栈

每次计算需要的符号都是从栈中获取的, 对于括号的求值, 处理方案是把括号去掉, 例如:

* `exp+(1+2)` 可以变为`exp+1+2`
* `exp-(1+2)` 可以变为`exp-1-2`

因为我们符号都是从栈中获取的, 所以把括号前的符号压入栈中就可以去括号了.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`

- - -

## Basic Calculator II

### 题目描述

> Implement a basic calculator to evaluate a simple expression string.
>
> The expression string contains only non-negative integers, `+`, `-`, `*`, `/` operators and empty spaces` `. The integer division should truncate toward zero.
>
> You may assume that the given expression is always valid.
>
> Some examples:
>
> `"3+2*2" = 7`
>
> `" 3/2 " = 1`
>
> `" 3+5 / 2 " = 5`
>
> Note: Do not use the eval built-in library function.

### 解法

代码如下:

    public static int calculate( String s ) {
        int num = 0;
        char preOp = '+';

        Stack<Integer> stack = new Stack<>();

        for( int i = 0; i < s.length(); ++i ) {
            char ch = s.charAt( i );
            if( Character.isDigit( ch ) ) {
                num = num * 10 + ch - '0';
            }
            if( ch=='+' || ch=='-' || ch=='*' || ch=='/'|| i==s.length()-1 ) {
                if( preOp == '+' ) {
                    stack.push( num );
                } else if( preOp == '-' ) {
                    stack.push( -num );
                } else if( preOp == '*' ) {
                    int num1 = stack.pop();
                    stack.push( num1 * num );
                } else if( preOp == '/' ) {
                    int num1 = stack.pop();
                    stack.push( num1 / num );
                }
                preOp = ch;
                num = 0;
            }
        }
        int res = 0;
        while( !stack.isEmpty() )
            res += stack.pop();
        return res;
    }

思考过程: 模拟实际运算过程.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`