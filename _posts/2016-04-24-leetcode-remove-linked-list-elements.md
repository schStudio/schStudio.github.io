---
layout: post
title: Remove Linked List Elements
category: Leetcode
---

* content
{:toc}

## Remove Linked List Elements

### 题目描述

> Remove all elements from a linked list of integers that have value val.
>
> Example
> 
> Given: `1 -> 2 -> 6 -> 3 -> 4 -> 5 -> 6`, `val = 6`
> 
> Return: `1 -> 2 -> 3 -> 4 -> 5`


### 解法

代码如下:

    public static ListNode removeElements( ListNode head, int val ) {
        ListNode dummyHead = new ListNode( 0 );
        dummyHead.next = head;
        ListNode curr = dummyHead;
        while( curr.next != null ) {
            if( curr.next.val == val ) {
                curr.next = curr.next.next;
                continue;
            }
            curr = curr.next;
        }
        return dummyHead.next;
    }

思考过程:

待删除元素可能在链表的任何一个位置, 包括链表头, 所以借助一个`dummyHead`来操作.

删除一个元素的方法就是把前一个元素的指针指向后一个元素. 使用`curr`表示前一个元素, 如果下一个元素为待删除元素, 那么就把`curr`指针指向后一个元素, 然后继续; 否则进入下一个节点.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`