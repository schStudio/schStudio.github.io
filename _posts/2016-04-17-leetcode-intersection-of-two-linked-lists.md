---
layout: post
title: Intersection of Two Linked Lists
category: Leetcode
---

* content
{:toc}

## Intersection of Two Linked Lists

### 题目描述

> Write a program to find the node at which the intersection of two singly linked lists begins.
>
>
> For example, the following two linked lists:
>
    A:          a1 → a2
                       ↘
                        c1 → c2 → c3
                       ↗
    B:     b1 → b2 → b3
>
> begin to intersect at node `c1`.
>
> Notes:
>
> * If the two linked lists have no intersection at all, return null.
> * The linked lists must retain their original structure after the function returns.
> * You may assume there are no cycles anywhere in the entire linked structure.
> * Your code should preferably run in `O(n)` time and use only `O(1)` memory.

### 解法

代码如下:

    public static ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode currA = headA, currB = headB;
        // 获取链表A和B的长度
        int lenA = 0, lenB = 0;
        while( currA != null ) { ++lenA; currA = currA.next; }
        while( currB != null ) { ++lenB; currB = currB.next; }
        // 让指针currA和currB指向平等的起始点
        currA = headA;
        currB = headB;
        if( lenA < lenB )
            while( lenB-- != lenA ) currB = currB.next;
        else
            while( lenA-- != lenB ) currA = currA.next;

        while( currA != currB ) {
            currA = currA.next;
            currB = currB.next;
        }
        return currA;
    }

思考过程: 如果两个链表的长度是一样的, 那么就可以使用两个指针一对一对的比较了, 但是链表的长度可能是不一样的, 所以可以想办法让两个指针指向的起始点是平等的, 也就是两个指针的起始点到终点的长度是相等的.

所以先计算两个链表的长度, 然后让较长链表的指针先移动一段距离, 使得和另一个指针处在平等的起始点, 最后一对一对比较.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`