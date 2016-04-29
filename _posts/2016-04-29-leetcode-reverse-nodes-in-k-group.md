---
layout: post
title: Reverse Nodes in k-Group
category: Leetcode
---

* content
{:toc}

## Reverse Nodes in k-Group

### 题目描述

> Given a linked list, reverse the nodes of a linked list `k` at a time and return its modified list.
> 
> If the number of nodes is not a multiple of `k` then left-out nodes in the end should remain as it is.
> 
> You may not alter the values in the nodes, only nodes itself may be changed.
> 
> Only constant memory is allowed.
> 
> For example,
> 
> Given this linked list: `1->2->3->4->5`
> 
> For `k = 2`, you should return: `2->1->4->3->5`
> 
> For `k = 3`, you should return: `3->2->1->4->5`

### 解法

代码如下:

    public static ListNode reverseKGroup( ListNode head, int k ) {
        ListNode dummyHead = new ListNode( 0 );
        dummyHead.next = head;
        ListNode tail = dummyHead;
        ListNode low = head, high = head;
        while( high != null ) {
            for( int i = 0; high != null && i < k - 1; ++i )
                high = high.next;
            if( high == null ) break;
            ListNode next = high.next;
            high.next = null;
            reverse( low );
            // 拼接
            tail.next = high;
            low.next = next;

            tail = low;
            low = high = next;
        }
        return dummyHead.next;
    }
    private static void reverse( ListNode head ) {
        ListNode pre = null, curr = head;
        while( curr != null ) {
            ListNode next = curr.next;
            curr.next = pre;
            pre = curr;
            curr = next;
        }
    }

思考过程: 

题目要求把长度为k的所有链表段进行翻转.

首先考虑链表头是否需要处理, 很明显是需要翻转的, 因此使用`dummyHead`排除链表头边界情况.

再者每次都要翻转长度为k的链表段, 先抽象出一个翻转的方法.

最后使用双指针, 头指针指向段头, 尾指针指向段尾, 找到所有长度为k的链表段, 分别翻转.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`