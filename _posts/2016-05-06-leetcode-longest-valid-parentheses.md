---
layout: post
title: Longest Valid Parentheses
category: Leetcode
---

* content
{:toc}

## Longest Valid Parentheses

### 题目描述

> Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.
> 
> For `"(()"`, the longest valid parentheses substring is `"()"`, which has length = `2`.
> 
> Another example is `")()())"`, where the longest valid parentheses substring is `"()()"`, which has length = `4`.

### 解法

代码如下:

    public static int longestValidParentheses(String s) {
        int[] dp = new int[s.length()];
        int max = 0;
        for(int i = 1; i < s.length(); i++) {
            char c = s.charAt(i);
            if(c == ')' && i - dp[i-1] - 1 >= 0 && s.charAt(i - dp[i-1] - 1) == '(') {
                dp[i] = 2 + dp[i-1] + (i - dp[i-1] - 2 >= 0 ? dp[i - dp[i-1] - 2] : 0);
                max = Math.max(max, dp[i]);
            }
        }
        return max;
    }

思考过程: `动态规划`，算法参考自[这里](https://leetcode.com/discuss/8092/my-dp-o-n-solution-without-using-stack)。

首先观察一个元素对结果可以产生什么影响，假设当前元素表示为`[i]`

* 如果`[i] == '('`, 那么该元素无法对结果产生任何影响
* 如果`[i] == '）'`, 那么该元素对结果可能产生影响，例如`“(())”, i == 3`

继续观察，可以发现可能产生影响的字符串格式如下：`“(有效()对)”`。也就是说跟左边邻接的有效括号对长度有关，因此定义状态：

* `dp[i]`：字符串`s[0...i]`以`[i]`结尾的有效括号对长度

那么，可以发现对于元素`[i]`：

* 如果`[i] == '('`，那么`dp[i] = 0`
* 如果`[i] == '）'`，那么`dp[i] = 2 + dp[i-1] + dp[i-dp[i]-2]`

字符串的格式是`"S1(S2)"`，其中：

* 数字2表示包围`S2`的左右括号长度
* `S1`表示左括号左边的有效括号对，值为`dp[i-dp[i]-2]`
* `S1`表示左右括号中间的有效括号对，值为`dp[i-1]`

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(n)`