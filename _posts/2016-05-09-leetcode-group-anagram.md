---
layout: post
title: Group Anagrams
category: Leetcode
---

* content
{:toc}

## Group Anagrams

### 题目描述

> You are given an `n x n` 2D matrix representing an image.
> 
> Rotate the image by `90` degrees (clockwise).
> 
> Follow up:
> 
> Could you do this in-place?

### 解法

代码如下:

> Given an array of strings, group anagrams together.
> 
> For example, given: `["eat", "tea", "tan", "ate", "nat", "bat"]`,
> 
> Return:
> 
    [
      ["ate", "eat","tea"],
      ["nat","tan"],
      ["bat"]
    ]
>
> Note:
> 
> 1. For the return value, each inner list's elements must follow the lexicographic order.
> 
> 2. All inputs will be in lower-case.

代码如下:

    public static List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<>();
        for(String s : strs) {
            char[] cstr = s.toCharArray();
            Arrays.sort(cstr);
            String key = new String(cstr);
            List<String> value;
            if((value = map.get(key)) != null) {
                value.add(s);
            } else {
                value = new ArrayList<>();
                value.add(s);
                map.put(key, value);
            }
        }
        List<List<String>> res = new ArrayList<>(map.values());
        for(List<String> list : res )
            Collections.sort(list);
        return res;
    }

思考过程: `哈希表+排序`。

题目要求把变位词归到一起，所以首先思考如何识别相同的变位词：相同变位词排序后相等。

其次思考如何快速找到排序后变位词对应的列表：哈希表存储键值对<排序的变位词，列表>。

通过以上两个步骤，就可以把相同的变位词快速的归到一起。

时空复杂度: 时间复杂度是`O(nmlogm)`, 空间复杂度是`O(nm)`

> 注：`m`代表最大单词长度