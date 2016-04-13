---
layout: post
title: Binary Search Tree Iterator
category: Leetcode
---

* content
{:toc}

## Binary Search Tree Iterator

### 题目描述

> Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.
>
> Calling `next()` will return the next smallest number in the BST.
>
> Note: 
>
> * `next()` and `hasNext()` should run in average `O(1)` time and uses `O(h)` memory, where `h` is the height of the tree.

### 解法

代码如下:

    class BSTIterator {

        Stack<TreeNode> s = new Stack<>();

        public BSTIterator(TreeNode root) {
            while( root != null ) {
                s.push( root );
                root = root.left;
            }
        }

        /** @return whether we have a next smallest number */
        public boolean hasNext() {
            return !s.isEmpty();
        }

        /** @return the next smallest number */
        public int next() {
            TreeNode node = s.pop();
            TreeNode right = node.right;
            while( right != null ) {
                s.push( right );
                right = right.left;
            }
            return node.val;
        }
    }

思考过程: 迭代器在这里等价于进行中序遍历, 使用栈实现中序遍历即可.