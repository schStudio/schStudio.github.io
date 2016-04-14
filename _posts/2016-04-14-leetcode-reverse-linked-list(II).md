---
layout: post
title: Reverse Linked List(II)
category: Leetcode
---

* content
{:toc}

## Reverse Linked List

### 题目描述

> Reverse a singly linked list.

### 解法

代码如下:

    public static ListNode reverseList( ListNode head ) {
        ListNode pre = null, curr = head;
        while( curr != null ) {
            ListNode next = curr.next;
            curr.next = pre;
            pre = curr;
            curr = next;
        }
        return pre;
    }

思考过程: 理解这两个变量的意义:

* `pre` : 维护了翻转链表的表头
* `curr` : 待处理的当前节点

处理过程每次把当前节点`curr`指向翻转链表的表头`pre`, 再更新`pre`和`curr`的值.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`

- - -

## Reverse Linked List II

### 题目描述

> Reverse a linked list from position m to n. Do it in-place and in one-pass.
>
> For example:
>
> Given `1->2->3->4->5->NULL`, `m = 2` and `n = 4`,
>
> return `1->4->3->2->5->NULL`.
>
> Note:
>
> Given `m`, `n` satisfy the following condition:
>
> `1 ≤ m ≤ n ≤ length of list`.

### 解法

代码如下:

    public static ListNode reverseList( ListNode head, int m, int n ) {
        ListNode dummyHead = new ListNode( 0 );
        dummyHead.next = head;
        // 移动到第m个节点
        ListNode pre = dummyHead, curr = head;
        for( int i = 1; i < m; ++i ) {
            pre = curr;
            curr = curr.next;
        }
        // 保留左部分链表的尾部和翻转链表的尾部
        ListNode leftTail = pre;
        ListNode reverseTail = curr;
        pre = curr;
        curr = curr.next;
        // 开始翻转
        while( curr != null && m++ < n ) {
            ListNode next = curr.next;
            curr.next = pre;
            pre = curr;
            curr = next;
        }
        // 拼接翻转链和右部分链表
        reverseTail.next = curr;
        leftTail.next = pre;

        return dummyHead.next;
    }

思考过程: 首先移动到第m个节点, 然后记录几个变量, 以便后面处理完拼接链表:

* `leftTail` : 左部分链表的尾部节点
* `reverseTail` : 翻转链表的尾部节点

经过翻转处理后, 开始拼接3条链表, 分别是左部分链表/翻转链表/右部分链表.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`