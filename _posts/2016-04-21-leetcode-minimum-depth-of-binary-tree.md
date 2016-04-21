---
layout: post
title: Minimum Depth of Binary Tree
category: Leetcode
---

* content
{:toc}

## Minimum Depth of Binary Tree

### 题目描述

> Given a binary tree, find its minimum depth.
>
> The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

### 解法

代码如下:

    public static int minDepth( TreeNode root ) {
        if( root == null ) return 0;
        if( root.left == null ) return minDepth( root.right ) + 1;
        if( root.right == null ) return minDepth( root.left ) + 1;
        return Math.min( minDepth( root.left ), minDepth( root.right ) ) + 1;
    }

思考过程: 

求一棵二叉树的最小路径长度:

* 如果二叉树为空, 返回`0`
* 如果二叉树不为空, 求左右子树的最小路径长度
* 如果左子树为空, 求右子树的最小路径长度+1
* 如果右子树为空, 求左子树的最小路径长度+1
* 如果左右子树都不为空, 求左右子树中较小的长度+1

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`