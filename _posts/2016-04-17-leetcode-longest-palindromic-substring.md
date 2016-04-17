---
layout: post
title: Longest Palindromic Substring
category: Leetcode
---

* content
{:toc}

## Longest Palindromic Substring

### 题目描述

> Given a string S, find the longest palindromic substring in S. You may assume that the maximum length of S is 1000, and there exists one unique longest palindromic substring.

### 解法

代码如下:

    public static String longestPalindrome( String s ) {
        // 转化
        StringBuilder sb = new StringBuilder();
        sb.append( '#' );
        for( int i = 0; i < s.length(); ++i )
            sb.append( s.charAt(i) ).append( '#' );
        // 结果
        int low = 0, high = 0, res = 0;
        // 右边界最长的中点j
        int j = 0;

        // len[i]: 中点i的最长回文串长度
        int[] len = new int[sb.length()];
        len[0] = 1;

        for( int i = 1; i < sb.length(); ++i ) {
            // 根据中点j得到len[i]的回文串长度(至少)
            if( j+len[j]/2 <= i ) 
                len[i] = 1;
            else
                len[i] = Math.min( len[2*j-i], len[j]-2*(i-j) );

            // 基于len[i]继续找最长回文串
            int start = i-len[i]/2, end = i+len[i]/2;
            while( start >= 0 && end < sb.length() &&
                    sb.charAt(start) == sb.charAt(end) ) {
                --start;
                ++end;
            }
            len[i] = end-start-1;

            // 更新结果
            if( len[i] > res ) {
                res = len[i];
                low = start+1;
                high = end-1;
            }

            // 更新中点j
            j = i+len[i]/2 > j+len[j]/2 ? i : j;
        }
        return s.substring( low / 2, high / 2 );
    }

思考过程: `Manacher's Algorithm`.

有关算法的思想请参考我的另一篇[博文](http://schstudio.github.io./2016/04/07/hihocoder-longest-palindromic-substring/).

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(n)`