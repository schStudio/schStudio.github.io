---
layout: post
title: Copy List with Random Pointer
category: Leetcode
---

* content
{:toc}

## Copy List with Random Pointer

### 题目描述

> A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.
>
> Return a deep copy of the list.

### 解法

代码如下:

    public static RandomListNode copyRandomList( RandomListNode head ) {
        Map<RandomListNode, RandomListNode> hash = new HashMap<>();
        RandomListNode dummyHead = new RandomListNode( 0 );
        // 单链表复制
        RandomListNode tail = dummyHead;
        RandomListNode curr = head;
        while( curr != null ) {
            RandomListNode node = new RandomListNode( curr.label );
            tail.next = node;
            tail = tail.next;
            hash.put( curr, node );
            curr = curr.next;
        }
        tail.next = null;
        // 随机指针复制
        curr = head;
        while( curr != null ) {
            RandomListNode node1 = hash.get( curr );
            RandomListNode node2 = hash.get( curr.random );
            node1.random = node2;
            curr = curr.next;
        }
        return dummyHead.next;
    }

思考过程: `哈希表`.

在单链表还没有完全构造出来之前无法构造每一个节点的随机指针, 所以第一步是复制单链表. 接下来再来构造随机指针, 构造随机指针需要知道原链表节点对所对应的新链表节点对, 所以可以在复制单链表的时候把原节点和新节点映射起来, 之后就可以用哈希表查询构造随机指针.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(n)`