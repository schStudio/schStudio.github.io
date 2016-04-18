---
layout: post
title: Valid Parentheses
category: Leetcode
---

* content
{:toc}

## Valid Parentheses

### 题目描述

> Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.
>
> The brackets must close in the correct order, `"()"` and `"()[]{}"` are all valid but `"(]"` and `"([)]"` are not.

### 解法

代码如下:

    public static boolean isValid( String s ) {
        Stack<Character> stack = new Stack<>();
        for( int i = 0; i < s.length(); ++i ) {
            char ch = s.charAt( i );
            switch( ch ) {
            case '(' :
            case '{' :
            case '[' :
                stack.push( ch );
                break;
            case ')' :
                if( stack.isEmpty() ) return false;
                if( stack.pop() != '(' ) return false;
                break;
            case '}' :
                if( stack.isEmpty() ) return false;
                if( stack.pop() != '{' ) return false;
                break;
            case ']' :
                if( stack.isEmpty() ) return false;
                if( stack.pop() != '[' ) return false;
                break;
            }
        }
        return stack.isEmpty();
	}

思考过程:

考虑一个有效的字符串`(...)`, 其中间部分必然两两配对消除, 而且`(`要先于中间任何字符, `)`要后于中间任何字符, 这具有`栈`的性质, 因为栈的性质是先入后出, 而这里的先字符与后字符匹配, 相当于先字符入栈, 遇到右字符出栈.

所以建立一个栈, 遇到左号就入栈, 遇到右号就出栈, 出栈后进行匹配判断.

注意字符串的形式有3种情况:

1. 有效字符串
2. 无效字符串, 左号个数多于右号个数
3. 无效字符串, 左号个数少于右号个数

所以在处理过程中要注意栈为空(情况3), 或者处理完栈不为空(情况2)

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`