---
layout: post
title: Sort List
category: Leetcode
---

* content
{:toc}

## Sort List

### 题目描述

> Sort a linked list in `O(nlogn)` time using constant space complexity.

### 解法

代码如下:

    public static ListNode sortList( ListNode head ) {

        if( head == null || head.next == null ) 
            return head;

        // 获取链表长度
        int len = 0;
        for( ListNode iNode = head; iNode != null; iNode = iNode.next )
            ++len;

        ListNode dummyHead = new ListNode( 0 );
        dummyHead.next = head;
        for( int step = 1; step < len; step <<= 1 ) {
            ListNode curr = dummyHead.next;
            ListNode tail = dummyHead;
            while( curr != null ) {
                ListNode left = curr;
                ListNode right = split( left, step );
                curr = split( right, step );
                tail = merge( left, right, tail );
            }
        }
        return dummyHead.next;
    }
    // 从head节点开始, 在第n个节点处切断, 返回第n个节点
    private static ListNode split( ListNode head, int n ) {
        for( int i = 1; head != null && i < n; ++i )
            head = head.next;
        if( head == null ) return null;
        ListNode next = head.next;
        head.next = null;
        return next;
    }
    // 把left和right归并到tail后面, 返回尾节点
    private static ListNode merge( ListNode left, ListNode right, ListNode tail ) {
        ListNode curr = tail;
        while( left != null && right != null ) {
            if( left.val < right.val ) {
                curr.next = left;
                left = left.next;
            } else {
                curr.next = right;
                right = right.next;
            }
            curr = curr.next;
        }
        curr.next = left == null ? right : left;
        while( curr.next != null ) curr = curr.next;
        return curr;
    }

思考过程: 一开始很容易想到归并排序, 但是用常规的`由顶向底`的递归方式来进行归并排序的话, 空间复杂度将会是`O(logn)`, 所以需要改用`由底向顶`的迭代方式进行归并排序.

时空复杂度: 时间复杂度是`O(nlogn)`, 空间复杂度是`O(1)`

- - -

下面给出递归版本 :

代码如下:

    public static ListNode sortList1( ListNode head ) {
        if( head == null || head.next == null )
            return head;

        ListNode dummyHead = new ListNode( 0 );
        dummyHead.next = head;
        ListNode slow = dummyHead, fast = dummyHead;
        while( fast != null && fast.next != null ) {
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode head1 = head, head2 = slow.next;
        slow.next = null;

        head1 = sortList( head1 );
        head2 = sortList( head2 );

        ListNode curr = dummyHead;
        while( head1 != null && head2 != null ) {
            if( head1.val < head2.val ) {
                curr.next = head1;
                head1 = head1.next;
            } else {
                curr.next = head2;
                head2 = head2.next;
            }
            curr = curr.next;
        }
        curr.next = head1 == null ? head2 : head1;
        return dummyHead.next;
    }