---
layout: post
title: Find the Duplicate Number
category: Leetcode
---

* content
{:toc}

## Find the Duplicate Number

### 题目描述

> Given an array nums containing `n + 1` integers where each integer is between `1` and `n` (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.
> 
> Note:
>
> * You must not modify the array (assume the array is read only).
> * You must use only constant, `O(1)` extra space.
> * Your runtime complexity should be less than `O(n2)`.
> * There is only one duplicate number in the array, but it could be repeated more than once.


### 解法

代码如下:

    public static int findDuplicate(int[] nums) {
        int slow = nums[0], fast = nums[nums[0]];
        while( slow != fast ) {
            slow = nums[slow];
            fast = nums[nums[fast]];
        }
        slow = 0;
        while( slow != fast ) {
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }

思考过程: 该题目的算法思想跟[Linked List Cycle II](http://schstudio.github.io/2016/04/15/leetcode-linked-list-cycle(II)/#linked-list-cycle-ii)一样.

暴力法 : 对于每一个元素, 在其余元素中查找是否重复. 时间复杂度为`O(n^2)`, 空间复杂度为`O(1)`.

排序法 : 对数组排序, 之后相邻两个元素对比查找是否重复. 时间复杂度为`O(nlogn)`, 空间复杂度为`O(1)`.

哈希法 : 使用哈希表记录每一个元素, 查找是否重复记录. 时间复杂度为`O(n)`, 空间复杂度为`O(n)`.

以上三种解决方案都被题目要求给否决了, 因此需要思考另外的解决方案.

从给出的条件特性入手, 已知数组中存在一个重复元素, 而且元素取值范围是`[1, n]`, 如果以元素下标作为节点, 元素值作为指针, 那么数组必定会形成一条带环的单链表, 而且重复元素就是环的入口点, 那么可以使用这道题[Linked List Cycle II](http://schstudio.github.io/2016/04/15/leetcode-linked-list-cycle(II)/#linked-list-cycle-ii)的解法.

下标`0`相当于单链表中的`dummyHead`.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`