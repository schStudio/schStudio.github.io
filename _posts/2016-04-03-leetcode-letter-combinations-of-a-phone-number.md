---
layout: post
title: Letter Combinations of a Phone Number
category: Leetcode
---

* content
{:toc}

## Letter Combinations of a Phone Number

### 题目描述

> Given a digit string, return all possible letter combinations that the number could represent.
>
> A mapping of digit to letters (just like on the telephone buttons) is given below.
>
> ![phone-number](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)
>
> Input:Digit string "23"
> 
> Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
> 
> Note:
> 
> Although the above answer is in lexicographical order, your answer could be in any order you want.

### 解法

代码如下:
		
    private static String[] digitRep = {
            " ", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz" };

    public static List<String> letterCombinatioins( String digits ) {
        List<String> res = new ArrayList<>();

        // 特殊情况单独处理
        if( "".equals(digits) ) 
            return res;

        helper( digits, 0, "", res );
        return res;
    }

    private static void helper( String digits, int t, 
            String tmpRes, List<String> res ) {
        if( t == digits.length() ) {
            res.add( tmpRes );
            return;
        }
        // 考虑每一个解空间
        String allLetter = digitRep[digits.charAt(t)-'0'];
        for( int i = 0; i < allLetter.length(); ++i ) {
            char ch = allLetter.charAt(i);
            helper( digits, t+1, tmpRes+ch, res );
        }
    }
        
思考过程: `回溯法`. 考虑每一个解空间即可, 没有任何剪枝

时空复杂度: 时间复杂度是O(3^n), 空间复杂度是O(n)