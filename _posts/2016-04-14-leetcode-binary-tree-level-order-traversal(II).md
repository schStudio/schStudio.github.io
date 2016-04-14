---
layout: post
title: Binary Tree Level Order Traversal
category: Leetcode
---

* content
{:toc}

## Binary Tree Level Order Traversal

### 题目描述

> Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).
>
> For example:
>
> Given binary tree `{3,9,20,#,#,15,7}`,
>
      3 
     / \
    9  20
      /  \
     15   7
>
> return its level order traversal as:
>
    [
      [3],
      [9,20],
      [15,7]
    ]

### 解法

代码如下:

    public static List<List<Integer>> levelOrder( TreeNode root ) {
        List<List<Integer>> res = new ArrayList<>();

        Queue<TreeNode> q1 = new LinkedList<>();
        Queue<TreeNode> q2 = new LinkedList<>();

        if( root != null ) q1.add( root );

        while( !q1.isEmpty() ) {
            List<Integer> tmpRes = new ArrayList<>();
            while( !q1.isEmpty() ) {
                TreeNode node = q1.poll();
                tmpRes.add( node.val );
                if( node.left != null ) q2.add( node.left );
                if( node.right != null ) q2.add( node.right );
            }
            res.add( tmpRes );
            Queue<TreeNode> q3 = q1;
            q1 = q2;
            q2 = q3;
        }
        return res;
    }

思考过程: `BFS`. 使用两个队列, 一个表示待处理的队列`q1`, 一个表示下一层的队列`q2`, 每次处理`q1`, 处理完就把下一层队列`q2`变为当前队列, 如此不断处理, 直到队列`q1`为空.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(n)`

- - -

## Binary Tree Level Order Traversal II

### 题目描述

> Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).
>
> For example:
>
> Given binary tree `{3,9,20,#,#,15,7}`,
>
      3 
     / \
    9  20
      /  \
     15   7
>
> return its bottom-up level order traversal as:
>
    [
      [3],
      [9,20],
      [15,7]
    ]

### 解法

代码如下:

    public static List<List<Integer>> levelOrder( TreeNode root ) {
        List<List<Integer>> res = new ArrayList<>();

        Queue<TreeNode> q1 = new LinkedList<>();
        Queue<TreeNode> q2 = new LinkedList<>();

        if( root != null ) q1.add( root );

        while( !q1.isEmpty() ) {
            List<Integer> tmpRes = new ArrayList<>();
            while( !q1.isEmpty() ) {
                TreeNode node = q1.poll();
                tmpRes.add( node.val );
                if( node.left != null ) q2.add( node.left );
                if( node.right != null ) q2.add( node.right );
            }
            res.add( tmpRes );
            Queue<TreeNode> q3 = q1;
            q1 = q2;
            q2 = q3;
        }
	    Collections.reverse( res );
        return res;
    }

思考过程: 同上一题, 最后把结果翻转.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(n)`