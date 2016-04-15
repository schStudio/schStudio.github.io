---
layout: post
title: Linked List Cycle(II)
category: Leetcode
---

* content
{:toc}

## Linked List Cycle

### 题目描述

> Given a linked list, determine if it has a cycle in it.
>
> Follow up:
> 
> Can you solve it without using extra space?

### 解法

代码如下:

    public static boolean hasCycle( ListNode head ) {
        ListNode dummyHead = new ListNode( 0 );
        dummyHead.next = head;

        ListNode slow = dummyHead, fast = dummyHead;
        while( fast != null && fast.next != null ) {
            slow = slow.next;
            fast = fast.next.next;
            if( slow == fast ) 
                return true;
        }
        return false;
    }

思考过程: 快慢指针. 慢指针每次走一步, 快指针每次走两步. 如果链表中存在环, 那么快慢指针一定会相遇, 如果链表中不存在环, 那么快指针一定会走到链表尽头.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`

- - -

## Linked List Cycle II

### 题目描述

> Given a linked list, return the node where the cycle begins. If there is no cycle, return null.
>
> Follow up:
> 
> Can you solve it without using extra space?

### 解法

代码如下:

    public static ListNode detectCycle( ListNode head ) {
        if( head == null || head.next == null ) return null;

        ListNode dummyHead = new ListNode( 0 );
        dummyHead.next = head;
        ListNode slow = head, fast = head.next;
        while( fast != null && fast.next != null ) {
            slow = slow.next;
            fast = fast.next.next;
            if( slow == fast ) 
                break;
        }
        if( slow != fast ) return null;
        slow = dummyHead;
        while( slow != fast ) {
            slow = slow.next;
            fast = fast.next;
        }
        return slow;
    }

思考过程: 快慢指针. 如果链表中不存在环, 那么很容易判断出来, 如果存在环, 如下图所示:

![leetcode-linked-list-cycle]({{ site.url }}/assets/leetcode/leetcode-linked-list-cycle.png)

* 一开始, 快慢指针都在`dummyHead`出发
* 当慢指针走到环的入口点时, 假设走了`m`的路程, 那么快指针就走了`2m`的路程
* 继续走快慢指针会相遇, 而且相遇点如图所示
* 最后只需要让慢指针从`dummyHead`出发, 快指针原地, 两个指针同步走直到相遇就是入口点

第一次相遇点的位置理解如下: 假设相遇点与入口点的距离为m, 下面证明假设是成立的:

* 慢指针走的路程是`m + n - 2m` => `n - m`
* 块指针走的路程是`n - 2m + m + m + n - 2m` => `2 * (n - m)`

因此假设是成立的.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`