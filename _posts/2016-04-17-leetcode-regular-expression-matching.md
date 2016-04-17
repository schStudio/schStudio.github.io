---
layout: post
title: Regular Expression Matching
category: Leetcode
---

* content
{:toc}

## Regular Expression Matching

### 题目描述

> Implement regular expression matching with support for `'.'` and `'*'`.
>
    '.' Matches any single character.
    '*' Matches zero or more of the preceding element.
>
    The matching should cover the entire input string (not partial).
>
    The function prototype should be:
    bool isMatch(const char *s, const char *p)
>
    Some examples:
    isMatch("aa","a") → false
    isMatch("aa","aa") → true
    isMatch("aaa","aa") → false
    isMatch("aa", "a*") → true
    isMatch("aa", ".*") → true
    isMatch("ab", ".*") → true
    isMatch("aab", "c*a*b") → true

### 解法

代码如下:

    public static boolean isMatch(String s, String p) {
        return helper(s, 0, p, 0);
    }

    private static boolean helper(String s, int si, String p, int pi) {
        if ( si == s.length() && pi == p.length() )
            return true;
        if ( pi + 1 < p.length() && p.charAt( pi+1 ) == '*' ) {
	        // 解空间为s[si+1...si+k]
            int k = 0;
            while ( si + k < s.length() && 
                    ( s.charAt( si+k ) == p.charAt( pi ) || p.charAt( pi ) == '.' ) ) {
                if ( helper( s, si+k+1, p, pi+2 ) )
                    return true;
                ++k;
            }
	        // 解空间为s[si]
            return helper( s, si, p, pi+2 );
        }
        // 限界函数
        if( si == s.length() || pi == p.length() )
            return false;

        if ( s.charAt( si ) == p.charAt( pi ) || p.charAt( pi ) == '.' ) {
            return helper( s, si+1, p, pi+1 );
        } else {
            return false;
        }
    }

思考过程: `回溯法`.

首先对匹配串的每一个单位(`字符`, `字符*`)进行分支处理:

* `字符*` : 字符可以取任意个, 所以其解空间为`[si...si+k]`, 也就是所有的可能空间都考虑
* `字符` : 判断当前字符是否匹配, 如果匹配进入下一层, 如果不匹配则匹配失败

> 注意要先判断`字符*`, 之后再判断限界函数, 因为`*`代表的字符可以取0个

时空复杂度: 时间复杂度是`未确定`, 空间复杂度是`未确定`