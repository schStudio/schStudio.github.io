---
layout: post
title: Merge Two(k) Sorted Lists
category: Leetcode
---

* content
{:toc}

## Merge Two Sorted Lists

### 题目描述

> Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

### 解法

代码如下:

    public static ListNode mergeTowLists( ListNode l1, ListNode l2 ) {
        ListNode dummyHead = new ListNode( 0 );
        ListNode curr = dummyHead;
        while( l1 != null && l2 != null ) {
            if( l1.val < l2.val ) {
                curr.next = l1;
                l1 = l1.next;
            } else {
                curr.next = l2;
                l2 = l2.next;
            }
            curr = curr.next;
        }
        curr.next = l1 == null ? l2 : l1;
        return dummyHead.next;
    }

思考过程: 借助一个`dummyHead`就可以使得循环处理流程一致, 简化代码.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`

- - -

## Merge k Sorted Lists

### 题目描述

> Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

### 解法

代码如下:

    public static ListNode mergeKLists( ListNode[] lists ) {
        if( lists.length == 0 ) return null;
        return helperMergeKLists( lists, 0, lists.length-1 );
    }

    private static ListNode helperMergeKLists( ListNode[] lists, int low, int high ) {
        if( low == high ) {
            return lists[low];
        }
        int mid = low + ( high - low ) / 2;
        ListNode l1 = helperMergeKLists( lists, low, mid );
        ListNode l2 = helperMergeKLists( lists, mid+1, high );
        return mergeTowLists( l1, l2 );
    }

思考过程: `分治法`. 借助[mergeTwoLists](#section-1), 加上分治法两两合并.

时空复杂度: 时间复杂度是`O(mlogn)`: `m`表示节点个数, 空间复杂度是`O(logn)`: `n`表示链表个数
