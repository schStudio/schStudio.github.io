---
layout: post
title: Palindrome Number
category: Leetcode
---

* content
{:toc}

## Palindrome Number

### 题目描述

> Determine whether an integer is a palindrome. Do this without extra space.
> 
> Some hints:
> 
> Could negative integers be palindromes? (ie, -1)
> 
> If you are thinking of converting the integer to string, note the restriction of using extra space.
> 
> You could also try reversing an integer. However, if you have solved the problem "Reverse Integer", you know that the reversed integer might overflow. How would you handle such case?
> 
> There is a more generic way of solving this problem.


### 解法

代码如下:

    public static boolean isPalindrome( int x ) {
        int y = x;
        int res = 0;
        while( y != 0 ) {
            res = res * 10 + y % 10;
            y /= 10;
            // 负数或溢出
            if( res < 0 ) return false;
        }
        return res == x;
    }

思考过程:

根据回文的性质是对称, 把数字翻转过来后进行比较, 如果相等那么就是`Palindrome Number`, 否则就不是.

但是还需要考虑负数, 经过测试发现题目假设负数不是`Palindrome Number`.

还需要注意的是在翻转数字的时候如果出现了值溢出, 那么可直接断定不是`Palindrome Number`

所以在翻转过程中如果出现负数, 就直接返回`false`.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`