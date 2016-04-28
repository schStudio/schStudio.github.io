---
layout: post
title: Remove Duplicates from Sorted List(II)
category: Leetcode
---

* content
{:toc}

## Remove Duplicates from Sorted List

### 题目描述

> Given a sorted linked list, delete all duplicates such that each element appear only once.
> 
> For example,
> 
> Given `1->1->2`, return `1->2`.
> 
> Given `1->1->2->3->3`, return `1->2->3`.

### 解法

代码如下:

    public static ListNode deleteDuplicates( ListNode head ) {
        if( head == null ) return null;
        ListNode curr = head;
        while( curr.next != null ) {
            if( curr.next.val == curr.val )
                curr.next = curr.next.next;
            else
                curr = curr.next;
        }
        return head;
    }

思考过程: 

使用`curr`指针指向非重复元素, 如果`curr`下一个元素与`curr.val`相等, 就把下一个元素删除, 删除之后`curr`还是指向当前元素; 否则`curr`指针指向下一个元素.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`

- - -

## Remove Duplicates from Sorted List II

### 题目描述

> Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.
> 
> For example,
> 
> Given `1->2->3->3->4->4->5`, return `1->2->5`.
> 
> Given `1->1->1->2->3`, return `2->3`.

### 解法

代码如下:

    public static ListNode deleteDuplicates( ListNode head ) {
        if( head == null || head.next == null ) return head;
        ListNode dummyHead = new ListNode( 0 );
        dummyHead.next = head;
        ListNode tail = dummyHead;
        while( tail.next != null && tail.next.next != null ) {
            ListNode repeatedNode = tail.next;
            while( repeatedNode.next != null && repeatedNode.next.val == repeatedNode.val )
                repeatedNode = repeatedNode.next;
            if( tail.next == repeatedNode )
                tail = tail.next;
            else
                tail.next = repeatedNode.next;
        }
        return dummyHead.next;
    }

思考过程: 

因为要删除所有的重复元素, 所以链表头可能也是重复元素, 所以借助`dummyHead`来避免边界情况.

使用`tail`指针指向当前非重复元素, 当剩下的元素少于两个的时候退出循环.

循环中, 使用`repeadtedNode`指向可能的重复元素, 如果`repeatedNode`指向的确实是重复元素, 那么删除重复元素这一段; 否则`tail`指针指向下一个非重复元素.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`