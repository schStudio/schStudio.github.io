---
layout: post
title: Add Two Numbers
category: Leetcode
---

* content
{:toc}

## Add Two Numbers

### 题目描述

> You are given two linked lists representing two non-negative numbers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.
>
> Input: (`2 -> 4 -> 3`) + (`5 -> 6 -> 4`)
> 
> Output: `7 -> 0 -> 8`

### 解法

代码如下:

    public static ListNode addTwoNumbers2( ListNode l1, ListNode l2 ) {
        ListNode dummyHead = new ListNode( 0 );
        ListNode curr = dummyHead;
        int carry = 0;
        while( l1 != null || l2 != null ) {
            int val1 = (l1 != null) ? l1.val : 0;
            int val2 = (l2 != null) ? l2.val : 0;
            int sum = val1 + val2 + carry;
            carry = sum / 10;
            curr.next = new ListNode( sum % 10 );
            curr = curr.next;
            l1 = (l1 != null) ? l1.next : null;
            l2 = (l2 != null) ? l2.next : null;
        }
        if( carry != 0 )
            curr.next = new ListNode( carry );
        return dummyHead.next;
    }

思考过程: 算法思想很直接, 重点在于代码的简洁性, 代码参考于[这里](https://leetcode.com/articles/add-two-numbers/)

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`
