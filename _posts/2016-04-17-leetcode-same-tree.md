---
layout: post
title: Same Tree
category: Leetcode
---

* content
{:toc}

## Same Tree

### 题目描述

> Given two binary trees, write a function to check if they are equal or not.
>
> Two binary trees are considered equal if they are structurally identical and the nodes have the same value.

### 解法

* 递归版

    代码如下:

        public static boolean isSameTreeRecursive( TreeNode p, TreeNode q ) {
            if( p == null && q == null ) return true;
            if( p == null || q == null ) return false;
            return p.val == q.val && 
                    isSameTreeRecursive( p.left, q.left ) && 
                    isSameTreeRecursive( p.right, q.right );
        }

* 迭代版

    代码如下:

        public static boolean isSameTreeIterative( TreeNode p, TreeNode q ) {
            Stack<TreeNode> stack = new Stack<>();
            stack.push( p );
            stack.push( q );
            while( !stack.isEmpty() ) {
                q = stack.pop();
                p = stack.pop();
                if( p == null && q == null ) continue;
                if( p == null || q == null ) return false;
                if( p.val != q.val ) return false;
                stack.push( p.left );
                stack.push( q.left );
                stack.push( p.right );
                stack.push( q.right );
            }
            return true;
        }

思考过程: 

* 递归版 : 考虑当前节点是否相等, 如果相等继续判断左右子树是否相等
* 迭代版 : 把递归版的`DFS`过程以栈的形式实现


时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(h)`