---
layout: post
title: Rotate List
category: Leetcode
---

* content
{:toc}

## Sort List

### 题目描述

> Given a list, rotate the list to the right by k places, where k is non-negative.
>
> For example:
>
> Given `1->2->3->4->5->NULL` and `k = 2`,
>
> return `4->5->1->2->3->NULL`.

### 解法

代码如下:

    public static ListNode rotateRight( ListNode head, int k ) {
        if( head == null ) return null;
        int len = 0;
        for( ListNode iNode = head; iNode != null; iNode = iNode.next )
            ++len;
        k %= len;

        ListNode p1 = head, p2 = head;
        while( k-- > 0 )
            p2 = p2.next;
        while( p2.next != null ) {
            p1 = p1.next;
            p2 = p2.next;
        }
        p2.next = head;
        head = p1.next;
        p1.next = null;
        return head;
    }

思考过程: 使用两个指针`p1`和`p2`, 这两个指针的距离始终是`k`, 然后不断移动这两个指针, 一直到结尾处, 然后进行相关的指针操作, 操作过程比较直接, 这里不解释.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`