---
layout: post
title: Insertion Sort List
category: Leetcode
---

* content
{:toc}

## Insertion Sort List

### 题目描述

> Sort a linked list using insertion sort.

### 解法

代码如下:

    public static ListNode insertionSortList( ListNode head ) {
        ListNode dummyHead = new ListNode( 0 );
        while( head != null ) {
            ListNode next = head.next;
            ListNode pre = dummyHead;
            while( pre.next != null && pre.next.val < head.val )
                pre = pre.next;
            head.next = pre.next;
            pre.next = head;
            head = next;
        }
        return dummyHead.next;
    }

思考过程: 

借助`dummyHead`, 避免插入位置是链表头的边界情况.

对于每个元素, 找到对应的插入位置, 插入即可.

时空复杂度: 时间复杂度是`O(n^2)`, 空间复杂度是`O(1)`