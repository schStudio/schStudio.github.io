---
layout: post
title: Generate Parentheses
category: Leetcode
---

* content
{:toc}

## Generate Parentheses

### 题目描述

> Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.
>
> For example, given `n = 3`, a solution set is:
>
> `"((()))"`, `"(()())"`, `"(())()"`, `"()(())"`, `"()()()"`

### 解法

代码如下:

    public static List<String> generatePerenthesis( int n ) {
        List<String> res = new ArrayList<>();
        helper( n, n, "", res );
        return res;
    }

    private static void helper( int left, int right, String tmpRes, List<String> res ) {
        // 结束条件
        if( left < 0 ) return; 

        // 获得结果
        if( left == 0 && right == 0 ) {
            res.add( tmpRes );
            return;
        }
        // 添加左括号
        helper( left-1, right, tmpRes+"(", res );
        // 添加右括号: 右括号不能少于左括号
        if( right > left ) {
            helper( left, right-1, tmpRes+")", res );
        }
    }

思考过程: `回溯法`. 

1. 定义状态: `左括号数量, 右括号数量`. 
2. 考虑状态的解空间:
	* `左括号数量 < 0` 表示无解
	* `左括号数量 == 0 && 右括号数量 == 0` 表示一个解
	* 添加左括号: 只要`左括号数量 > 0`就可以
	* 添加右括号: 满足条件`右括号数量 > 左括号数量`就可以, 这就是`剪枝`

> 注:
>
> * `剪枝`动作如果放在`求解解空间`, 那么在`结束条件`那里就只考虑正常的结束条件
> * `剪枝`动作如果放在`结束条件`, 那么在`求解解空间`那里就无条件进所有子解空间

时空复杂度: 时间复杂度是`O(2^n)`, 空间复杂度是`O(n)`