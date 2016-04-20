---
layout: post
title: Validate Binary Search Tree
category: Leetcode
---

* content
{:toc}

## Validate Binary Search Tree

### 题目描述

> Given a binary tree, determine if it is a valid binary search tree (BST).
>
> Assume a BST is defined as follows:
>
> * The left subtree of a node contains only nodes with keys less than the node's key.
> * The right subtree of a node contains only nodes with keys greater than the node's key.
> * Both the left and right subtrees must also be binary search trees.

### 解法

* 递归版

    代码如下:

        public static boolean isValidBSTRecursive( TreeNode root ) {
            if( root == null ) return true;
            // 判断左子树最大值与根节点的值
            TreeNode left = root.left;
            while( left != null && left.right != null ) left = left.right;
            if( left != null && left.val >= root.val ) return false;
            // 判断右子树最小值与根节点的值
            TreeNode right = root.right;
            while( right != null && right.left != null ) right = right.left;
            if( right != null && right.val <= root.val ) return false;

            return isValidBSTRecursive( root.left ) && isValidBSTRecursive( root.right );
        }

* 迭代版

    代码如下:

        public static boolean isValidBSTIterative( TreeNode root ) {
            if( root == null ) return true;
            Stack<TreeNode> stack = new Stack<>();
            stack.push( root );
            while( !stack.isEmpty() ) {
                TreeNode node = stack.pop();
                if( node == null ) continue;
                // 判断左子树最大值与根节点的值
                TreeNode left = node.left;
                while( left != null && left.right != null ) left = left.right;
                if( left != null && left.val >= node.val ) return false;
                // 判断右子树最小值与根节点的值
                TreeNode right = node.right;
                while( right != null && right.left != null ) right = right.left;
                if( right != null && right.val <= node.val ) return false;

                stack.push( node.left );
                stack.push( node.right );
            }
            return true;
        }

思考过程: 

判断一棵二叉树是否为二分查找树 :

* 如果二叉树为空, 返回`true`
* 如果二叉树不为空
* 找到左子树的最大值进行合法性比较
* 找到右子树的最小值进行合法性比较
* 如果有一个不合法, 返回`false`
* 判断左子树是否为二分查找树
* 判断右子树是否为二分查找树
* 如果左右子树存在不合法, 返回`false`
* 否则二叉树就是二分查找树, 返回`true`

根据上面的条件就可以写出递归版和迭代版的代码.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`