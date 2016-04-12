---
layout: post
title: Swap Nodes in Pairs
category: Leetcode
---

* content
{:toc}

## Swap Nodes in Pairs

### 题目描述

> Given a linked list, swap every two adjacent nodes and return its head.
>
> For example,
>
> Given `1->2->3->4`, you should return the list as `2->1->4->3`.
>
> Note :
> 
> * Your algorithm should use only constant space. 
> * You may not modify the values in the list, only nodes itself can be changed.

### 解法

代码如下:

    public static ListNode swapPairs( ListNode head ) {
        ListNode dummyHead = new ListNode( 0 );
        dummyHead.next = head;
        ListNode curr = dummyHead;
        while( curr.next != null && curr.next.next != null ) {
            ListNode next1 = curr.next;
            ListNode next2 = next1.next;
            next1.next = next2.next;
            next2.next = next1;
            curr.next = next2;
            curr = next1;
        }
        return dummyHead.next;
    }

思考过程: 算法没有什么特别的, 就是找到后两个节点, 然后更改彼此之间的关系. 但是需要注意:

* 借助多一个`dummyHead`可以让代码更简洁
* 尽量把处理过程中需要的变量定义在处理循环里面, 这样代码更简洁也不容易出错.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`
