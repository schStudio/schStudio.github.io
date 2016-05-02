---
layout: post
title: Partition List
category: Leetcode
---

* content
{:toc}

## Partition List

### 题目描述

> Given a linked list and a value `x`, partition it such that all nodes less than `x` come before nodes greater than or equal to `x`.
> 
> You should preserve the original relative order of the nodes in each of the two partitions.
> 
> For example,
> 
> Given `1->4->3->2->5->2` and `x = 3`,
>
> return `1->2->2->4->3->5`.

### 解法

代码如下:

    public static ListNode partition(ListNode head, int x) {
        ListNode dummyA = new ListNode(0), tailA = dummyA;
        ListNode dummyB = new ListNode(0), tailB = dummyB;
        while(head != null) {
            ListNode next = head.next;
            if(head.val < x) {
                tailA.next = head;
                tailA = tailA.next;
            } else {
                tailB.next = head;
                tailB = tailB.next;
            }
            head = next;
        }
        tailB.next = null;
        tailA.next = dummyB.next;
        return dummyA.next;
    }

思考过程:

分别维护两条链表:

* `dummyA`: 元素值都小于指定值`x`
* `dummyB`: 元素值都大于等于指定值`x`

对原始链表每一个元素归类, 如果元素值小于指定值`x`, 归类到`dummyA`链表; 否则归类到`dummyB`链表.

最后拼接链表`dummyA`和链表`dummyB`, 然后返回.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`