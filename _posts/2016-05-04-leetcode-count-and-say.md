---
layout: post
title: Count and Say
category: Leetcode
---

* content
{:toc}

## Count and Say

### 题目描述

> The count-and-say sequence is the sequence of integers beginning as follows:
> `1`, `11`, `21`, `1211`, `111221`, `...`
> 
> `1` is read off as `"one 1"` or `11`.
> `11` is read off as `"two 1s"` or `21`.
> `21` is read off as `"one 2, then one 1"` or `1211`.
> Given an integer `n`, generate the nth sequence.
> 
> Note: The sequence of integers will be represented as a string.

### 解法

代码如下:

    public static String countAndSay(int n) {
        String res = "1";
        for(int i = 1; i < n; i++) {
            int low = 0, high = 1;
            String tmp = "";
            while(high <= res.length()) {
                while(high < res.length() && res.charAt(high) == res.charAt(high-1)) 
                    high++;
                int count = high - low;
                tmp = tmp + count + res.charAt(low);
                low = high++;
            }
            res = tmp;
        }
        return res;
    }

思考过程:

按照规律模拟生成.

时空复杂度: 时间复杂度是`O(n^2)`, 空间复杂度是`O(n^2)`
