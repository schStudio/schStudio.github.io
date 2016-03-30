---
layout: post
title: Permutations(II)
category: Leetcode
---

* content
{:toc}

## Permutations

### 题目描述

> Given a collection of distinct numbers, return all possible permutations.
> 
> For example,
> 
> [1,2,3] have the following permutations:
> 
> [1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], and [3,2,1].

### 解法

代码如下:
		
    static List<List<Integer>> res = new ArrayList<>();
    
    public static List<List<Integer>> permute( int[] nums ) {
        helperPermute( nums, 0 );
        return res;
    }
    private static void helperPermute( int[] nums, int t ) {
        if( t == nums.length ) {
            // 构造一个结果
            List<Integer> tmpRes = new ArrayList<>(nums.length);
            for( int num : nums ) tmpRes.add( num );
            
            // 加入结果集
            res.add( tmpRes );
        } else {
            for( int i = t; i < nums.length; ++i ) {
                swap( nums, t, i);
                helperPermute( nums, t+1 );
                swap( nums, t, i);
            }
        }
    }
    
    private static void swap( int[] nums, int i, int j ) {
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }
        
思考过程: 求一个集合的全排列, 这个问题可以归类为`回溯法`中的`排列树`问题. 

代码可以这样理解, 集合S的全排列由以下的结果组成:

* S[1]+全排列( S[2...n] )
* S[2]+全排列( S[1, 3...n] )
* S[3]+全排列( S[2, 1, 4...n] )
* ...
* S[n]+全排列( S[1...n-1] )

也就是把S的中每一个元素都提取到第一位, 剩下的元素做全排列

> 疑难点: 为什么要swap2次? 因为我们要保证在处理完一次结果之后, 要恢复数组的原始状态, 这样下一次处理的时候才能正确把下一个元素交换到第一个位置来.
    
时空复杂度: 时间复杂度是O(n!), 空间复杂度是O(n): 递归的空间, 排除构造出来的全排列

### 总结

> 如果一个处理过程是迭代形式的, 而且迭代过程有严格的状态要求, 那么就十分需要注意子处理过程所带来的影响.

- - -

## Permutations II

### 题目描述

> Given a collection of numbers that might contain duplicates, return all possible unique permutations.
> 
> For example,
> 
> [1,1,2] have the following unique permutations:
> 
> [1,1,2], [1,2,1], and [2,1,1].

### 解法

    static List<List<Integer>> res = new ArrayList<>();
    
    public static List<List<Integer>> permuteUnique( int[] nums ) {
        Arrays.sort( nums );
        helperPermuteUnique( nums, 0, nums.length-1 );
        return res;
    }
    private static void helperPermuteUnique( int[] nums, int i, int j ) {
        if( i == j ) {
            List<Integer> tmpRes = new ArrayList<>(nums.length);
            for( int num : nums ) tmpRes.add( num );
            res.add( tmpRes );
        } 
        for( int k = i; k <= j; ++k ) {
            if( findSame(nums, i, k) ) continue;
            swap( nums, i, k );
            helperPermuteUnique( nums, i+1, j );
            swap( nums, i, k );
        }
    }
    
    private static boolean findSame( int[] nums, int i, int j ) {
        for( int k = i; k < j; ++k )
            if( nums[j] == nums[k] ) return true;
        return false;
    }
    
    private static void swap( int[] nums, int i, int j ) {
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }

思考过程: 对于有重复元素集合的全排列, 可以用上面无重复元素的全排列来解决, 区别就是我们在这个基础上还要排除一些重复的全排列, 那么我们需要思考的问题是: 按照无重复元素的全排列处理过程, 如何消除重复元素带来的影响? 我们知道全排列的一次处理过程就是把每一个元素都搬到第一个位置, 然后做剩余元素的全排列, 所以我们只需要判断当前元素跟前面的元素有没有重复, 有重复的话就是已经处理过的, 直接跳过.

### 总结

> 借助已有的处理模型去思考问题的解决方案