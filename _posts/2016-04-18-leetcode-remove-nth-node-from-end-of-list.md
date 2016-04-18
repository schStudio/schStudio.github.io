---
layout: post
title: Remove Nth Node From End of List
category: Leetcode
---

* content
{:toc}

## Remove Nth Node From End of List

### 题目描述

> Given a linked list, remove the nth node from the end of list and return its head.
>
> For example,
>
> Given linked list: `1->2->3->4->5`, and `n = 2`.
>
> After removing the second node from the end, the linked list becomes `1->2->3->5`.
> Note:
>
> * Given n will always be valid.
> * Try to do this in one pass.

### 解法

代码如下:

    public static ListNode removeNthFromEnd( ListNode head, int n ) {
        ListNode dummyHead = new ListNode( 0 );
        dummyHead.next = head;
        // curr指针走n+1步
        ListNode curr = dummyHead;
        while( n-- >= 0 )
            curr = curr.next;
        // pre与curr相差n+1步, 最后pre指向第n+1个节点(倒数)
        ListNode pre  = dummyHead;
        while( curr != null ) {
            curr = curr.next;
            pre = pre.next;
        }
        // 删除倒数第n个节点
        pre.next = pre.next.next;
        return dummyHead.next;
    }

思考过程:

删除倒数第`n`个节点, 需要得到倒数第`n+1`个节点, 定义以下两个变量:

* `curr` : 当前指针, 用于标识走到链表结尾
* `pre` : 与`curr`指针相差`n+1`的距离, 当`curr`指针走到链表结尾时, `pre`指针指向倒数第n+1个节点

根据以上两个变量遍历链表, 得到倒数第`n+1`个节点, 最后删除倒数第`n`个节点.


时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`