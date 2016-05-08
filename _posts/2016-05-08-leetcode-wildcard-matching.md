---
layout: post
title: Wildcard Matching
category: Leetcode
---

* content
{:toc}

## Wildcard Matching

### 题目描述

> Implement wildcard pattern matching with support for '?' and '*'.
>
    '?' Matches any single character.
    '*' Matches any sequence of characters (including the empty sequence).
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
    isMatch("aa", "*") → true
    isMatch("aa", "a*") → true
    isMatch("ab", "?*") → true
    isMatch("aab", "c*a*b") → false

### 解法

代码如下:

    public static boolean isMatch(String s, String p) {
        int i = 0, j = 0, match = 0, startIndex = -1;
        while(i < s.length()) {
            if(j < p.length() && (p.charAt(j) == '?' || s.charAt(i) == p.charAt(j))) {
                i++;
                j++;
            } else if(j < p.length() && p.charAt(j) == '*') {
                startIndex = j;
                match = i;
                j++;
            } else if(startIndex != -1) {
                i = match++;
                j = startIndex + 1;
            } else {
                return false;
            }
        }
        while(j < p.length() && p.charAt(j) == '*')
            j++;
        return j == p.length();
    }

思考过程: `贪心法`，该算法参考自[这里](https://leetcode.com/discuss/10133/linear-runtime-and-constant-space-solution)。

使用两个指针`i`，`j`分别指向匹配串和模式串的当前字符，使用`match`指向已匹配成功的位置，`startIndex`指向前面最近的`*`。

考虑模式p当前字符的取值情况：

* 普通字符：直接与匹配串的当前字符比较，相等就把两个指针指向下一个，不相等就把`j`返回前面最近的`*`位置，并且把已匹配成功的位置下移一位，指针`i`从匹配成功的下一个位置开始
* `？`：肯定与匹配串的当前字符相等，把两个指针指向下一个
* `*`：更新`match`与`startIndex`，并且把`j`指向下一个

这里的算法只需要考虑前面最近的`*`位置，这是因为满足贪婪的性质（前面匹配成功的部分不会影响结果）例如：

* 匹配串：`S1...S2...[x]`
* 模式串：`P1 * P2 * [y]`

其中`S1`与`P1`，`S2`与`P2`是匹配的，`[x]`和`[y]`是当前字符，而且不匹配。

那么把`[y]`回退到第二个`*`比回退到第一个`*`更好，因为`P2`是已经成功匹配的了，再考虑进来是不会影响后面的匹配结果。

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`