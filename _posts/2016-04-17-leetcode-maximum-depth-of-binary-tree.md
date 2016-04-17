---
layout: post
title: Maximum Depth of Binary Tree
category: Leetcode
---

* content
{:toc}

## Maximum Depth of Binary Tree

### 题目描述

> Given a binary tree, find its maximum depth.
>
> The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

### 解法

代码如下:

    public static int maxDepth( TreeNode root ) {
        if( root == null ) return 0;
        return Math.max( maxDepth( root.left ), maxDepth( root.right ) ) + 1;
    }

思考过程: 首先判断节点是否存在, 不存在返回0; 存在就找出左子树和右子树深度中的最大值+1.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(h)`