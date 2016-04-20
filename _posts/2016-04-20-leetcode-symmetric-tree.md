---
layout: post
title: Symmetric Tree
category: Leetcode
---

* content
{:toc}

## Symmetric Tree

### 题目描述

> Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).
>
> For example, this binary tree is symmetric:
>
        1
       / \
      2   2
     / \ / \
    3  4 4  3
>
> But the following is not:
> 
        1
       / \
      2   2
       \   \
       3    3
>
> Note:
> Bonus points if you could solve it both recursively and iteratively.

### 解法

* 递归版

    代码如下:

        public static boolean isSymmetricRecursive( TreeNode root ) {
            if( root == null ) return true;
            return helper( root.left, root.right );
        }
        private static boolean helper( TreeNode left, TreeNode right ) {
            if( left == null && right == null )
                return true;
            if( left == null || right == null )
                return false;
            return left.val == right.val &&
                    helper( left.left, right.right ) &&
                    helper( left.right, right.left );
        }

* 迭代版

    代码如下:

        public static boolean isSymmetricIterative( TreeNode root ) {
            if( root == null ) return true;
            Stack<TreeNode> stack = new Stack<>();
            stack.push( root.left );
            stack.push( root.right );
            while( !stack.isEmpty() ) {
                TreeNode node1 = stack.pop();
                TreeNode node2 = stack.pop();
                if( node1 == null && node2 == null ) continue;
                if( node1 == null || node2 == null ) return false;
                if( node1.val != node2.val ) return false;
                stack.push( node1.left ); stack.push( node2.right );
                stack.push( node1.right ); stack.push( node2.left );
            }
            return true;
        }

思考过程: 

一棵二叉树如果是对称的, 要么这棵树本身为空, 要么这棵树的左子树和右子树是对称的.

因此定义一个判断两棵树`(a, b)`是否对称的方法 :

* 如果两棵树都为空, 返回`true`
* 如果有一棵为空一棵不为空, 返回`false`
* 如果`a.val != b.val`, 返回`false`
* 判断`(a.left, b.right)`是否对称
* 判断`(a.right, b.left)`是否对称
* 如果对应的子树也都对称, 返回`true`

根据上面的条件就可以写出递归版和迭代版的代码.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`