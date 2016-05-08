---
layout: post
title: Jump Game(II)
category: Leetcode
---

* content
{:toc}

## Jump Game

### 题目描述

> Given an array of non-negative integers, you are initially positioned at the first index of the array.
> 
> Each element in the array represents your maximum jump length at that position.
> 
> Determine if you are able to reach the last index.
> 
> For example:
> 
> `A = [2,3,1,1,4]`, return `true`.
> 
> `A = [3,2,1,0,4]`, return `false`.

### 解法

代码如下:

    public static boolean canJump(int[] nums) {
        int reach = 0;
        for(int i = 0; i < nums.length; i++) {
            if(reach < i) return false;
            reach = Math.max(reach, i + nums[i]);
            if(reach >= nums.length - 1) return true;
        }
        return reach >= nums.length - 1;
    }

思考过程: `贪心法`。

使用变量`reach`表示当前状态可以跳到的最远距离。

初始状态下，可以跳到的最远距离是0，所以`reach`的初始值是0。

接下来遍历每一个位置，如果`reach`的值到达不了当前位置，那么就肯定跳不到最后了，返回`false`；如果`reach`的值可以到达当前位置，那么更新`reach`看是否可以跳的更远。

最后查看`reach`是否可以到达最后。

在遍历的过程中可以加入一个判断，查看是否已经跳到最后了，如果跳到最后，那么后面的可以不用考虑了，返回`true`。

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`

- - -

## Jump Game II

### 题目描述

> Given an array of non-negative integers, you are initially positioned at the first index of the array.
> 
> Each element in the array represents your maximum jump length at that position.
> 
> Your goal is to reach the last index in the minimum number of jumps.
> 
> For example:
> 
> Given array `A = [2,3,1,1,4]`
> 
> The minimum number of jumps to reach the last index is `2`. (Jump `1` step from index `0` to `1`, then `3` steps to the last index.)
> 
> Note:
> 
> You can assume that you can always reach the last index.

### 解法

代码如下:

    public static int jump(int[] nums) {
        int count = 0, at = 0;
        while(at < nums.length - 1) {
            // 最后一跳
            if(at + nums[at] >= nums.length - 1) {
                count++;
                break;
            }
            // 搜索下一跳：使得可以跳的更远
            int next = at + 1;
            for(int i = at + 2, j = at + nums[at]; i <= j; i++)
                if(i + nums[i] >= next + nums[next])
                    next = i;
            at = next;
            count++;
        }
        return count;
    }

思考过程: `贪心法`。

定义变量`at`表示当前位置，`count`表示目前的跳数。

对于当前位置，搜索下一跳中能够跳的更远的位置，然后进入下一跳，计数加1。

为了方便，在最后一跳能够到达的时候退出搜索下一跳。

时空复杂度: 时间复杂度是`O(n^2)`, 空间复杂度是`O(1)`