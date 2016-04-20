---
layout: post
title: Serialize and Deserialize Binary Tree
category: Leetcode
---

* content
{:toc}

## Serialize and Deserialize Binary Tree

### 题目描述

> Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.
>
> Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.
>
> For example, you may serialize the following tree
>
        1
       / \
      2   3
         / \
        4   5
>
> as `"[1,2,3,null,null,4,5]"`, just the same as how LeetCode OJ serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.
> Note: Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

### 解法

* 序列化

    代码如下:

        public String serialize(TreeNode root) {
            Queue<TreeNode> queue = new LinkedList<>();
            StringBuilder sb = new StringBuilder();
            queue.add( root );
            while( !queue.isEmpty() ) {
                TreeNode node = queue.poll();
                if( node == null ) {
                    sb.append( '#' );
                    continue;
                }
                sb.append( node.val ).append( '#' );
                queue.add( node.left );
                queue.add( node.right );
            }
            return sb.toString();
        }

* 反序列化

    代码如下:

        public TreeNode deserialize(String data) {
            Queue<TreeNode> queue = new LinkedList<>();
            int low = 0, high = 0;
            // 获取根节点
            while( high < data.length() && data.charAt( high ) != '#' ) ++high;
            TreeNode root = evalNode( data.substring( low, high ) );
            high = low = high + 1;

            queue.add( root );
            while( high < data.length() ) {
                // 获取左子树根节点
                while( high < data.length() && data.charAt( high ) != '#' ) ++high;
                TreeNode left = evalNode( data.substring( low, high ) );
                // 获取右子树根节点
                high = low = high + 1;
                while( high < data.length() && data.charAt( high ) != '#' ) ++high;
                TreeNode right = evalNode( data.substring( low, high ) );
                // 左右子树根节点连接父节点
                TreeNode parent = queue.poll();
                parent.left = left;
                parent.right = right;
                // 加入队列
                if( left != null ) queue.add( left );
                if( right != null ) queue.add( right );
                // 进入下一对
                high = low = high + 1;
            }
            return root;
        }
        private TreeNode evalNode( String s ) {
            Integer val = "".equals( s ) ? null : Integer.parseInt( s );
            return val != null ? new TreeNode( val ) : null;
        }

思考过程:

对二叉树进行序列化和反序列化. 首先要定义一个规则, 按照规则进行序列化和反序列化就可以了, 规则如下:

* 对于非空节点, 序列化的结果表示为: `val#`
* 对于空节点, 序列化的结果表示为: `#`
* 以层次遍历的顺序把节点序列化和反序列化

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`