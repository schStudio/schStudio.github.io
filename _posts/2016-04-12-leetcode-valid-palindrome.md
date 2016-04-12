---
layout: post
title: Valid Palindrome
category: Leetcode
---

* content
{:toc}

## Valid Palindrome

### 题目描述

> Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.
>
> For example,
>
> `"A man, a plan, a canal: Panama"` is a palindrome.
>
> `"race a car"` is not a palindrome.
>
> Note:
>
> * Have you consider that the string might be empty? This is a good question to ask during an interview.
> * For the purpose of this problem, we define empty string as valid palindrome.

### 解法

代码如下:

    public static boolean isPalindrome( String s ) {
        char[] str = s.toCharArray();
        for( int low = 0, high = str.length-1; low < high; ++low, --high ) {
            while( low < high && 
                    !Character.isAlphabetic( str[low] ) && 
                    !Character.isDigit( str[low] ) ) ++low;
            while( low < high && 
                    !Character.isAlphabetic( str[high] ) && 
                    !Character.isDigit( str[high] ) ) --high;
            if( Character.toLowerCase( str[low] ) != 
                    Character.toLowerCase( str[high] ) )
                return false;
        }
        return true;
    }

思考过程: 定义两个指针:

* `low` : 处于字符串的低位, 指向低位中的第一个字母或数字
* `high` : 处于字符串的高位, 指向高位中的最后一个字母或数字

对`low`指针和`high`指针指向的元素进行判断, 一旦出现不等, 直接返回`false`; 如果全部判断都相等, 那么返回`true`.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`
