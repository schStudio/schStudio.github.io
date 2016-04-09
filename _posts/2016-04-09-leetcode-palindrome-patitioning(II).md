---
layout: post
title: Palindrome Partitioning(II)
category: Leetcode
---

* content
{:toc}

## Palindrome Partitioning

### 题目描述

> Given a string s, partition s such that every substring of the partition is a palindrome.
>
> Return all possible palindrome partitioning of `s`.
>
> For example, given `s = "aab"`,
>
> Return
>
    [
      ["aa","b"],
      ["a","a","b"]
    ]

### 解法

代码如下:

    public static List<List<String>> partition( String s ) {
        List<List<String>> res = new ArrayList<>();
        helperPartition( s, res, new ArrayList<String>() );
        return res;
    }

    private static void helperPartition( String s, 
            List<List<String>> res, List<String> tmpRes ) {
        if( "".equals( s ) ) {
            res.add( new ArrayList<String>( tmpRes ) );
            return;
        }

        for( int i = 1; i <= s.length(); ++i ) {
            String subStr = s.substring(0, i);
            if( isPalindrome( subStr ) ) {
                tmpRes.add( subStr );
                helperPartition( s.substring(i), res, tmpRes );
                tmpRes.remove( tmpRes.size()-1 );
            }
        }
    }
    public static boolean isPalindrome( String str ) {
        int n = str.length();
        for( int i = 0; i < n/2; ++i )
            if( str.charAt(i) != str.charAt(n-1-i) ) return false;
        return true;
    }


思考过程: `回溯法`.

求一个字符串的所有回文串切分, 需要先定义回文串这个名词和切分这个动作:

* 回文串: 回文串是指一个字符串顺着读和逆着读是一样的, 也即具有对称性质
* 切分: 一个切分是指把一个字符串分割为两部分

原题意中的切分是任意次的切分, 我们先考虑一次切分的情况: 一次切分把字符串切分为左部分和右部分. 如果左部分不是回文串, 此次切分没有解, 如果左部分是回文串, 那么原始问题可以定义为以下的形式:

* 字符串S表示待切分的字符串
* S的回文串切分 = S左部分 + S右部分的回文串切分

如此定义出来之后就可以遍历一个字符串的所有切分, 然后进行回溯判断即可.

时空复杂度: 时间复杂度为`O(n!)`, 空间复杂度为`O(n)`, `n`表示字符串的长度

- - -

## Palindrome Partitioning II

### 题目描述

> Given a string s, partition s such that every substring of the partition is a palindrome.
>
> Return the minimum cuts needed for a palindrome partitioning of `1`.
>
> For example, given `s = "aab"`,
> 
> Return 1 since the palindrome partitioning `["aa","b"]` could be produced using `1` cut.

### 解法

代码如下:

    public static int minCut( String s ) {
        int strLen = s.length();

        // dp[i]: 字符串在位置[0...i]的minCut
        int[] dp = new int[strLen];

        // pal[i][j]: [i...j]是否回文串
        boolean[][] pal = new boolean[strLen][strLen];

        for( int i = 0; i < strLen; ++i ) {
            dp[i] = i+1;
            for( int j = 0; j <= i; ++j ) {
                if( isPalindrome( pal, s, j, i ) ) {
                    pal[j][i] = true;
                    dp[i] = j==0 ? 0 : Math.min( dp[i], 1+dp[j-1] );
                }
            }
        }
        return dp[strLen-1];
    }
    private static boolean isPalindrome( boolean[][] pal, String s, int low, int high ) {
        return s.charAt(low) == s.charAt(high) && 
                ( low+1 > high-1 || pal[low+1][high-1] );
    }

思考过程: `动态规划`.

求字符串`[1...n]`的最小切割`F(1, n)`, 如果`[k...n], 1<=k<=n`是回文串, 则结果为`F(1, k-1) + 1`; 如果不是回文串, 那么此种情况就不可能是解. 所以定义:

* `dp[i]` : 字符串`[1...i]`的最小切割

求字符串`dp[i+1]`的时候, 其解空间为`{ min( dp[k-1] + 1 ) || 0, 2<=k<=i+1 }`. 等于`0`表示字符串`[1...i+1]`本身就是回文串, 不需要切割.

以上的算法中, 每次都需要判断字符串`[k+1, i+1]`是否是回文串, 判断算法的时间复杂度最小为`O(n)`, 这样并不是很好, 我们可以借助回文串的对称性质得到更优的判断算法.

* 如果`[i...j]`是回文串, 则`[i+1...j-1]`也必定是回文串
* 反过来, 如果`[i+1...j-1]`是回文串且`[i] == [j]`, 则`[i...j]`也是回文串

通过以上两条性质的分析, 我们定义:

* `pal[i][j]` : 字符串`[i...j]`是否为回文串

借助`pal[i][j]`我们可以以`O(1)`的时间复杂度判断回文串, 但是空间复杂度为`O(n^2)`


时空复杂度: 时间复杂度为`O(n^2)`, 空间复杂度为`O(n^2)`, `n`表示字符串的长度