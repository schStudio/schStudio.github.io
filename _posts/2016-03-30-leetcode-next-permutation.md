---
layout: post
title: Next Permutation
category: Leetcode
---

* content
{:toc}

### 题目描述

> Implement next permutation, which rearranges numbers into the lexicographically > next greater permutation of numbers.
> 
> If such arrangement is not possible, it must rearrange it as the lowest possible > order (ie, sorted in ascending order).
>
> The replacement must be in-place, do not allocate extra memory.
>
> Here are some examples. Inputs are in the left-hand column and its corresponding > outputs are in the right-hand column.
> 
> 1,2,3 → 1,3,2
> 
> 3,2,1 → 1,2,3
> 
> 1,1,5 → 1,5,1

### 解法

代码如下:

	public static void nextPermutation( int[] nums ) {

        // 从后往前找到第一个递增对 [i, i+1]
        int i = nums.length-2;
        for( ; i >= 0 && nums[i] >= nums[i+1]; --i );

        // 已经不存在递增对, 所有排列已经列举完了, 这里重新开始
        if( i < 0 ) {
            reverse( nums, 0, nums.length-1 );
            return;
        }

        // 从后往前找到第一个 [j] > [i]
        int j = nums.length-1;
        for( ; j > i && nums[j] <= nums[i]; --j );

        // 构造下一个全排列
        swap( nums, i, j );
        i = i + 1;
        j = nums.length-1;
        reverse( nums, i, j );
    }
    private static void swap( int[] nums, int i, int j ) {
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }
    private static void reverse( int[] nums, int i, int j ) {
        while( i < j ) {
            swap( nums, i++, j-- );
        }
    }

思考过程: 求一个排列的下一个排列, 从数值上看, 就是找到下一个值, 使得下一个值`比当前值大`并且要`最接近`. 所以我们可以从数值的低位部分入手, 因为改变低位对数值大小的影响最小, 也就满足了`最接近`这个条件. 现在考虑一个例子: 

* [A<sub>1</sub>, A<sub>2</sub>, ... , A<sub>K</sub>, A<sub>K+1</sub>, ..., A<sub>N</sub>]
* A<sub>K+1</sub> > ... > A<sub>N</sub> ( 1 < K < N, 也就是[K+1...N]递减 )

由于[K+1...N]这部分是递减的, 那么在这部分做修改是不可能构造出一个比当前值还大的值, 所以就要第[K]位加进来, 也就是对[K...N]这部分做修改, 那么该如何改呢? 

1. 在[K+1...N]中找一个刚好比[K]大的值, 与[K]交换, 才能构造出一个比当前值大的值, 所以就可以在[K+1...N]中由后往前找合适的值([K+1...N]是递减的, 所以由后往前找)
2. 找到合适的值之后与[K]交换, 交换之后[K+1...N]还是递减的
3. 修改[K+1...N]这部分变为最小值, 因为[K+1...N]是递减的, 翻转以下就可以了
4. 得到下一个全排列的值

> 疑难点: 求下一个排列的问题可以转化为数值的问题, 然后又要想到数值的变化影响因素中, 低位的影响是最小的, 所以思考点要转移到如何修改低位部分
    
时空复杂度: 时间复杂度是O(n), 空间复杂度是O(1)

### 总结

> 专注问题的思考点, 并且不断转换思考点, 直到问题清晰可解