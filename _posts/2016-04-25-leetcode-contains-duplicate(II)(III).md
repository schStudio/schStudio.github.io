---
layout: post
title: Contains Duplicate(II)(III)
category: Leetcode
---

* content
{:toc}

## Contains Duplicate

### 题目描述

> Given an array of integers, find if the array contains any duplicates. Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.


### 解法

代码如下:

    public static boolean containsDuplicate( int[] nums ) {
        Set<Integer> hash = new HashSet<>();
        for( int num : nums ) {
            if( hash.contains( num ) )
                return true;
            else
                hash.add( num );
        }
        return false;
    }

思考过程: 

暴力法: 对于每一个元素, 在剩余元素中查找是否重复. 时间复杂度为`O(n^2)`, 空间复杂度为`O(1)`.

排序法: 对数组排序, 比较相邻元素查找是否重复. 时间复杂度为`O(nlogn)`, 空间复杂度为`O(1)`.

哈希法: 对于每一个元素, 在哈希表中查找是否重复. 时间复杂度为`O(n)`, 空间复杂度为`O(n)`.

- - -

## Contains Duplicate II

### 题目描述

> Given an array of integers and an integer `k`, find out whether there are two distinct indices `i` and `j` in the array such that `nums[i]` = `nums[j]` and the difference between `i` and `j` is at most `k`.

### 解法

代码如下:

    public static boolean containsNearbyDuplicate(int[] nums, int k) {
        Set<Integer> set = new HashSet<>(k+1);
        for( int i = 0; i < nums.length; ++i ) {
            if( set.contains(nums[i]) ) {
                return true;
            } else if( set.size() < k ) {
                set.add( nums[i] );
            } else {
                set.add( nums[i] );
                set.remove( nums[i-k] );
            }
        }
        return false;
    } 

思考过程: 

暴力法: 对于每一个元素, 考虑前`k`个元素, 查找是否有重复. 时间复杂度为`O(nk)`, 空间复杂度为`O(1)`.

哈希法: 构建一个哈希表, 维护前`k`个元素的值, 对每一个元素查找是否重复. 时间复杂度为`O(n)`, 空间复杂度为`O(k)`.

- - -

## Contains Duplicate III

### 题目描述

> Given an array of integers, find out whether there are two distinct indices `i` and `j` in the array such that the difference between `nums[i]` and `nums[j]` is at most `t` and the difference between `i` and `j` is at most `k`.


### 解法

代码如下:

    public static boolean containsNearbyAlmostDuplicate( 
            int[] nums, int k, int t ) {
        TreeSet<Integer> set = new TreeSet<>();
        for( int i = 0; i < nums.length; ++i ) {
            Integer floor = set.floor( nums[i] + t );
            Integer ceil = set.ceiling( nums[i] - t );
            if( floor != null && floor >= nums[i] )
                return true;
            if( ceil != null && ceil <= nums[i] )
                return true;
            set.add( nums[i] );
            if( i >= k )
                set.remove( nums[i-k] );
        }
        return false;
    }

思考过程: 该算法参考自[这里](https://leetcode.com/discuss/38177/java-o-n-lg-k-solution).

暴力法: 对于每一个元素, 考虑前`k`个元素, 查找是否有重复. 时间复杂度为`O(nk)`, 空间复杂度为`O(1)`.

二叉查找树: 构建一个二叉查找树, 维护前`k`个元素的值, 查找范围限制内的值并判断是否重复. 时间复杂度为`O(nlogk)`, 空间复杂度为`O(k)`.