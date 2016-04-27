---
layout: post
title: Convert Sorted List to Binary Search Tree
category: Leetcode
---

* content
{:toc}

## Convert Sorted List to Binary Search Tree

### 题目描述

> Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

### 解法

代码如下:

    public static TreeNode sortedListToBST( ListNode head ) {
        if( head == null ) return null;
        if( head.next == null ) return new TreeNode( head.val );

        ListNode dummyHead = new ListNode( 0 );
        dummyHead.next = head;
        ListNode slow = head, fast = head.next;
        ListNode tail = dummyHead;
        while( fast != null && fast.next != null ) {
            tail = slow;
            slow = slow.next;
            fast = fast.next.next;
        }
        tail.next = null;
        TreeNode root = new TreeNode( slow.val );
        root.left = sortedListToBST( dummyHead.next );
        root.right = sortedListToBST( slow.next );
        return root;
    }

思考过程: 算法思想与这题[Convert Sorted Array to Binary Search Tree](http://schstudio.github.io./2016/04/27/leetcode-convert-sorted-array-to-binary-search-tree/)一样.

这里寻找范围内中间值的方式为快慢指针.

还需要注意链表的切割位置, 因为中间值是作为根节点的, 所以链表的切割位置就是中间值的左邻居, 因此代码中需要借助`tail`变量来记录中间值的左邻居.

但是如果链表头就是范围内的中间值, 其左邻居就无法表示, 因此又借助`dummyHead`来排除边界情况.

时空复杂度: 时间复杂度是`O(nlogn)`, 空间复杂度是`O(logn)`