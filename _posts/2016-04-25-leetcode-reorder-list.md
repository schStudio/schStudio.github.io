---
layout: post
title: Reorder List
category: Leetcode
---

* content
{:toc}

## Patching Array

### 题目描述

> Given a singly linked list `L`: `L0→L1→…→Ln-1→Ln`,
> reorder it to: `L0→Ln→L1→Ln-1→L2→Ln-2→…`
> 
> You must do this in-place without altering the nodes' values.
> 
> For example,
> 
> Given `{1,2,3,4}`, reorder it to `{1,4,2,3}`.


### 解法

代码如下:

    public static void reorderList( ListNode head ) {
        if( head == null || head.next == null )
            return;
        ListNode slow = head, fast = head.next;
        while( fast != null && fast.next != null ) {
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode h1 = head, h2 = reverseList( slow.next );
        slow.next = null;
        while( h2 != null ) {
            ListNode h2next = h2.next;
            h2.next = h1.next;
            h1.next = h2;
            h1 = h2.next;
            h2 = h2next;
        }
    }
    private static ListNode reverseList( ListNode head ) {
        ListNode pre = null, curr = head;
        while( curr != null ) {
            ListNode next = curr.next;
            curr.next = pre;
            pre = curr;
            curr = next;
        }
        return pre;
    }

思考过程: 

首先考虑节点个数为奇偶的情况:

* 节点个数为奇数 : 链表右半段插入左半段, 例如`{1, 2, 3} => {1, 3, 2}`
* 节点个数为偶数 : 链表右半段插入左半段, 例如`{1, 2, 3, 4} => {1, 4, 2, 3}`

也就是说, 无论节点个数为奇偶, 算法是一样的, 没有额外的边界情况. 但是注意平分左右半段的时候, 如果多出一个节点要归到左半段, 而使用快慢指针恰好满足这种情况.

算法处理流程流程如下:

* 使用快慢指针找到切分点, 把链表切成左半段和右半段
* 翻转右半段
* 把右半段插入左半段

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`