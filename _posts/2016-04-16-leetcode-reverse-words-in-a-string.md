---
layout: post
title: Reverse Words in a String
category: Leetcode
---

* content
{:toc}

## Reverse Words in a String

### 题目描述

> Given an input string, reverse the string word by word.
>
> For example,
>
> Given `s = "the sky is blue"`,
>
> return `"blue is sky the"`.

### 解法

代码如下:

    public static String reverseWords( String s ) {
        int ws = 0, we = 0;
        int len = s.length();
        StringBuilder sb = new StringBuilder();
        while( we < len ) {
            while( we < len && s.charAt(we) == ' ' ) ++we;
            ws = we;
            while( we < len && s.charAt(we) != ' ' ) ++we;
            for( int i = we-1; i >= ws; --i )
                sb.append( s.charAt(i) );
            sb.append( ' ' );
        }
        return new StringBuilder( sb.toString().trim() ).reverse().toString();
    }

思考过程: 每次获取一个单词, 翻转形式追加到结果, 最后对整个字符串翻转, 但是要注意先去除尾部的空白符.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(n)`