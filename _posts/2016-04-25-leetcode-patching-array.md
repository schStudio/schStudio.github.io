---
layout: post
title: Patching Array
category: Leetcode
---

* content
{:toc}

## Patching Array

### 题目描述

> Given a sorted positive integer array nums and an integer n, add/patch elements to the array such that any number in range `[1, n]` inclusive can be formed by the sum of some elements in the array. Return the minimum number of patches required.
> 
> Example 1:
> 
> `nums = [1, 3]`, `n = 6`
> 
> Return `1`.
> 
> Combinations of nums are `[1], [3], [1,3]`, which form possible sums of: `1, 3, 4`.
> 
> Now if we add/patch `2` to nums, the combinations are: `[1], [2], [3], [1,3], [2,3], [1,2,3]`.
> 
> Possible sums are `1, 2, 3, 4, 5, 6`, which now covers the range `[1, 6]`.
> 
> So we only need `1` patch.
> 
> Example 2:
> 
> `nums = [1, 5, 10]`, `n = 20`
> 
> Return `2`.
> 
> The two patches can be `[2, 4]`.
> 
> Example 3:
> 
> `nums = [1, 2, 2]`, `n = 5`
> 
> Return `0`.


### 解法

代码如下:

    public static int minPatches( int[] nums, int n ) {
        long lost = 1;
        int count = 0, i = 0;
        while( lost <= n ) {
            if( i < nums.length && nums[i] <= lost ) {
                lost += nums[i++];
            } else {
                lost += lost;
                ++count;
            }
        }
        return count;
    }

思考过程: 该算法参考自[这里](https://leetcode.com/discuss/82822/solution-explanation)

要得到某范围内的所有组合值, 先根据给出的数组求出所有组合值, 然后对于剩余的值进行筛选, 容易发现较大的值可以由较小的值组合得到, 而较小的值则不可能由其他值得到.

因此这里的筛选策略就必然是`贪心`: 每次找到目前无法得到的最小值.

下面给出算法的几个变量定义:

* `lost` : lost表示当前无法得到最小值, 也就是范围`[0, lost)`的值可以得到
* `count` : 结果计数器
* `i` : 数组`nums`的下标

处理过程中从已有数组元素入手:

* 如果数组元素已经得到, 那么可以扩大组合值, 使得取值范围增大.

* 如果数组元素无法得到, 那么`lost`就是当前无法得到的最小值, 计数器增加, 取得`lost`并增大取值范围

最后要注意可能发生溢出的情况, 所以`lost`变量需要使用`long`类型.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`