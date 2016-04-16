---
layout: post
title: Majority Element(II)
category: Leetcode
---

* content
{:toc}

## Majority Element

### 题目描述

> Given an array of size n, find the majority element. The majority element is the element that appears more than `⌊ n/2 ⌋` times.
>
> You may assume that the array is non-empty and the majority element always exist in the array.

### 解法

代码如下:

    public static int majorityElement( int[] nums ) {
        int res = 0, count = 0;
        for( int i = 0; i < nums.length; ++i ) {
            if( nums[i] == res )
                ++count;
            else
                --count;

            if( count < 0 ) {
                res = nums[i];
                count = 1;
            }
        }
        return res;
    }

思考过程: 根据题意可以知道`Majority Element`就是个数大于等于一半的元素, 根据这个条件定义一个变量`count`表示保留元素的个数, 对于`Majority Element`的`count`一定大于等于0. 所以遍历所有元素, 如果当前元素与保留元素一致, 增加`count`, 如果不一致, 减少`count`, 如果`count`小于0, 那么当前元素不是`Majority Element`, 更换保留元素.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`

- - -

## Majority Element II

### 题目描述

> Given an integer array of size n, find all elements that appear more than `⌊ n/3 ⌋` times. The algorithm should run in linear time and in `O(1)` space.

### 解法

代码如下:

    public static List<Integer> majorityElement( int[] nums ) {
        List<Integer> res = new ArrayList<>();
        int res1 = 0, count1 = 0;
        int res2 = 1, count2 = 0;
        for( int i = 0; i < nums.length; ++i ) {
            if( nums[i] == res1 ) {
                ++count1;
            } else if( nums[i] == res2 ) {
                ++count2;
            } else if( count1 == 0 ) {
                res1 = nums[i];
                count1 = 1;
            } else if( count2 == 0 ) {
                res2 = nums[i];
                count2 = 1;
            } else {
                --count1;
                --count2;
            }
        }
        count1 = count2 = 0;
        for( int i = 0; i < nums.length; ++i ) {
            if( nums[i] == res1 ) ++count1;
            if( nums[i] == res2 ) ++count2;
        }
        if( count1 > nums.length / 3 ) res.add( res1 );
        if( count2 > nums.length / 3 ) res.add( res2 );
        return res;
    }

思考过程: [Boyer-Moore Majority Vote Algorithm](https://en.wikipedia.org/wiki/Boyer%E2%80%93Moore_majority_vote_algorithm)

模拟投票过程就比较容易理解, 投票数超过`1/3`表明最多只有两个候选人, 那么定义两个候选人和候选人的票数, 如果新的投票是给候选人的, 那么只要把对应候选人的票数增加, 如果不是投给任何候选人的, 那么等价于把候选人的票数分别减少.

整个过程就是找出具有可能性的候选人, 最后再验证一次是否成立.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`
