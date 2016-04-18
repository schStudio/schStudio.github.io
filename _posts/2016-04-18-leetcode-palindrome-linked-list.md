---
layout: post
title: Palindrome Linked List
category: Leetcode
---

* content
{:toc}

## Palindrome Linked List

### 题目描述

> Given a singly linked list, determine if it is a palindrome.
>
> Follow up:
>
> Could you do it in O(n) time and O(1) space?

### 解法

代码如下:

    public static boolean isPalindrome( ListNode head ) {
        ListNode dummyHead = new ListNode( 0 );
        dummyHead.next = head;
        // 找到链表中点
        ListNode slow = dummyHead, fast = dummyHead;
        while( fast != null && fast.next != null ) {
            slow = slow.next;
            fast = fast.next.next;
        }
        // 翻转中点之后的链表
        ListNode pre = slow, curr = slow.next;
        while( curr != null ) {
            ListNode next = curr.next;
            curr.next = pre;
            pre = curr;
            curr = next;
        }
        // 判断是否回文链表
        ListNode headA = head, headB = pre;
        while( headA != headB ) {
            if( headA.val != headB.val )
                return false;
            headA = headA != headB ? headA.next : headA;
            headB = headB != headA ? headB.next : headB;
        }
        return true;
    }

思考过程: 

因为链表不支持随机访问, 所以暴力解法就是对于每一节点都遍历一次链表找到对称的节点进行比较, 如此一来时间复杂度会是`O(n^2)`.

再多思考一点, 其实回文串就是具有对称性质, 那么可以改造链表使其具有对称性质, 容易想到只要把链表中点之后的部分翻转过来, 那么链表就具有对称性质了, 因此算法过程如下:

1. 使用快慢指针找到链表的中点
2. 翻转链表中点之后的右半部分
3. 判断回文串

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`