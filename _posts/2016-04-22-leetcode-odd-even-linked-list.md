---
layout: post
title: Odd Even Linked List
category: Leetcode
---

* content
{:toc}

## Odd Even Linked List

### 题目描述

> Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.
>
> You should try to do it in place. The program should run in `O(1)` space complexity and `O(nodes)` time complexity.
>
> Example:
>
> Given `1->2->3->4->5->NULL`,
>
> return `1->3->5->2->4->NULL`.
>
> Note:
>
> * The relative order inside both the even and odd groups should remain as it was in the input. 
> * The first node is considered odd, the second node even and so on ...

### 解法

代码如下:

    public static ListNode oddEvenList( ListNode head ) {
        ListNode dummyHead1 = new ListNode( 0 );
        ListNode dummyHead2 = new ListNode( 0 );
        ListNode tail1 = dummyHead1;
        ListNode tail2 = dummyHead2;

        ListNode oddNode = head;
        while( oddNode != null && oddNode.next != null ) {
            ListNode evenNode = oddNode.next;
            tail1.next = oddNode;
            tail2.next = evenNode;
            tail1 = tail1.next;
            tail2 = tail2.next;
            oddNode = evenNode.next;
        }

        if( oddNode != null ) {
            tail1.next = oddNode;
            tail1.next = tail1.next;
        }
        tail2.next = null;
        tail1.next = dummyHead2.next;
        return dummyHead1.next;
    }

官网参考代码如下:

    public ListNode oddEvenList(ListNode head) {
        if (head == null) return null;
        ListNode odd = head, even = head.next, evenHead = even;
        while (even != null && even.next != null) {
            odd.next = even.next;
            odd = odd.next;
            even.next = odd.next;
            even = even.next;
        }
        odd.next = evenHead;
        return head;
    }

思考过程: 

使用两个`dummyHead`, 一个用于保存奇数链表, 一个用于保存偶数链表, 最后把奇数链表和偶数链表连接起来, 返回即可.

官网的做法是原始链表表示奇数链表, 然后用一个指针指向一个偶数链表. 每次操作都是把奇数节点和偶数节点归到两条链表, 最后把奇数链表和偶数链表连接返回.

> 官网的代码值得学习

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`