---
layout: post
title: Repeated DNA Sequences
category: Leetcode
---

* content
{:toc}

## Repeated DNA Sequences

### 题目描述

> All DNA is composed of a series of nucleotides abbreviated as `A`, `C`, `G`, and `T`, for example: `"ACGAATTCCG"`. When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.
> 
> Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.
> 
    For example,
>
    Given s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT",
>
    Return:
    ["AAAAACCCCC", "CCCCCAAAAA"].

### 解法

代码如下:

    public static List<String> findRepeatedDnaSequences(String s) {
        List<String> res = new ArrayList<>();
        Map<String, Integer> map = new HashMap<>();
        int low = 0, high = 9;
        while(high < s.length()) {
            String substr = s.substring(low, high + 1);
            if(map.containsKey(substr)) {
                int count = map.get(substr); 
                if(count == 1) res.add(substr);
                map.put(substr, count + 1);
            } else {
                map.put(substr, 1);
            }
            low++;
            high++;
        }
        return res;
    }

思考过程:

题目要求查询重复的DNA子序列, 那么可以使用哈希表保存已经查询过的DNA子序列, 后面的DNA子序列根据哈希表可以快速查询. 查询的时候:

* 如果没有重复, 加入哈希表
* 如果重复了1次, 该DNA子序列加入结果集
* 如果重复多于1次, 那么不需要重复加入结果集

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(n)`