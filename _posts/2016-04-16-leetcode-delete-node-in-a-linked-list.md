---
layout: post
title: Delete Node in a Linked List
category: Leetcode
---

* content
{:toc}

## Delete Node in a Linked List

### 题目描述

> Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.
>
> Supposed the linked list is `1 -> 2 -> 3 -> 4` and you are given the third node with value `3`, the linked list should become `1 -> 2 -> 4` after calling your function.

### 解法

代码如下:

    public static void deleteNode( ListNode node ) {
        node.val = node.next.val;
        node.next = node.next.next;
    }

思考过程: 算法思想就是节点转移, 把待删除节点转移到下一个节点进行删除就可以了.

时空复杂度: 时间复杂度是`O(1)`, 空间复杂度是`O(1)`