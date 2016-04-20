---
layout: post
title: Balanced Binary Tree
category: Leetcode
---

* content
{:toc}

## Balanced Binary Tree

### 题目描述

> Given a binary tree, determine if it is height-balanced.
>
> For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

### 解法

* 自顶向底

    代码如下:

        public static boolean isBalanced( TreeNode root ) {
            if( root == null ) return true;
            int leftHeight = treeHeight( root.left );
            int rightHeight = treeHeight( root.right );
            return Math.abs( leftHeight - rightHeight ) <= 1 &&
                    isBalanced( root.left ) &&
                    isBalanced( root.right );
        }
        private static int treeHeight( TreeNode root ) {
            if( root == null ) return 0;
            return Math.max( treeHeight( root.left ), 
                             treeHeight( root.right ) ) + 1; 
        }

* 自底向顶

    代码如下:

        public static boolean isBalancedDFS( TreeNode root ) {
            return height( root ) != -1;
        }
        private static int height( TreeNode root ) {
            if( root == null ) return 0;
            int leftH = height( root.left );
            if( leftH == -1 ) return -1;
            int rightH = height( root.right );
            if( rightH == -1 ) return -1;
            if( Math.abs( leftH - rightH ) > 1 ) return -1;
            return Math.max( leftH , rightH ) + 1;
        }

思考过程: 

一棵平衡的二叉树, 左子树的高与右子树的高相差小于等于1, 并且左子树和右子树都是平衡树.

因此很容易想到以下的解决步骤:

* 如果二叉树是空树, 返回`true`
* 如果二叉树非空
* 比较左子树和右子树的高
* 如果相差大于1, 返回`false`
* 再检查左子树和右子树是否平衡
* 如果任意子树不平衡, 返回`false`

以上解决方案可行, 但是会重复计算子树的高, 导致效率很差.

反过来思考, 只要任意子树出现不平衡, 那么其祖先也必定不平衡, 因此解决方案就变为了`自底向顶`.

做法是在求树高的时候顺便检查树是否平衡, 如果树非平衡, 我们返回非平衡状态`-1`, 如此级联形式返回, 就不会出现多余的运算检查.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`