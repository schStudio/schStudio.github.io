---
layout: post
title: Bulls and Cows
category: Leetcode
---

* content
{:toc}

## Find Peak Element

### 题目描述

> You are playing the following Bulls and Cows game with your friend: You write down a number and ask your friend to guess what the number is. Each time your friend makes a guess, you provide a hint that indicates how many digits in said guess match your secret number exactly in both digit and position (called "bulls") and how many digits match the secret number but locate in the wrong position (called "cows"). Your friend will use successive guesses and hints to eventually derive the secret number.
>
> For example:
>
> Secret number:  `"1807"`
>
> Friend's guess: `"7810"`
>
> Hint: `1` bull and `3` cows. (The bull is `8`, the cows are `0, 1 and 7`.)
>
> Write a function to return a hint according to the secret number and friend's guess, use `A` to indicate the bulls and `B` to indicate the cows. In the above example, your function should return `"1A3B"`.
>
> Please note that both secret number and friend's guess may contain duplicate digits, for example:
>
> Secret number:  `"1123"`
>
> Friend's guess: `"0111"`
>
> In this case, the 1st `1` in friend's guess is a bull, the 2nd or 3rd 1 is a cow, and your function should return `"1A1B"`.
>
> You may assume that the secret number and your friend's guess only contain digits, and their lengths are always equal.

### 解法

代码如下:

    public static String getHint( String secret, String guess ) {
        int[] hash = new int[10];
        int countA = 0, countB = 0;
        // 统计bulls
        for( int i = 0; i < secret.length(); ++i ) {
            char cha = secret.charAt( i );
            char chb = guess.charAt( i );
            if( cha == chb )
                ++countA;
            else
                ++hash[cha-'0'];
        }
        // 统计cows
        for( int i = 0; i < guess.length(); ++i ) {
            char c = guess.charAt(i);
            if( c != secret.charAt( i ) && hash[c-'0'] > 0 ) {
                ++countB;
                --hash[c-'0'];
            }
        }
        return countA + "A" + countB + "B";
    }

思考过程: `哈希表`.

bulls的统计和cows的统计要分别开来, 因为满足bulls与满足cows之间是冲突的, 而且bulls的统计优先级要高于cows的, 所以第一步统计bulls, 第二步再统计cows.

在第一步统计bulls的过程中, 使用一个哈希表来记录非bulls的数字

在第二步统计cows的过程中, 借助第一步的哈希表来统计


时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`