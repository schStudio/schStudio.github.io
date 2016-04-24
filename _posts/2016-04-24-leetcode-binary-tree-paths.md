---
layout: post
title: Binary Tree Paths
category: Leetcode
---

* content
{:toc}

## Binary Tree Paths

### 题目描述

> Given a binary tree, return all root-to-leaf paths.
>
> For example, given the following binary tree:
>
       1
     /   \
    2     3
     \
      5
>
> All root-to-leaf paths are:
>
> `["1->2->5", "1->3"]`



### 解法

递归版代码如下:

    public static List<String> binaryTreePaths( TreeNode root ) {
        List<String> res = new ArrayList<>();
        if( root == null ) return res;
        helper( root, root.val+"", res );
        return res;
    }

    private static void helper( TreeNode root, String tmpRes, List<String> res ) {
        if( root.left == null && root.right == null ) {
            res.add( tmpRes );
            return;
        }
        if( root.left != null )
            helper( root.left, tmpRes+"->"+root.left.val, res );
        if( root.right != null )
            helper( root.right, tmpRes+"->"+root.right.val, res );
    }

迭代版代码如下:

    public static List<String> binaryTreePathsIterative( TreeNode root ) {
        List<String> res = new ArrayList<>();
        if( root == null ) return res;

        Stack<TreeNode> stack = new Stack<>();
        Stack<String> tmpRes = new Stack<>();
        stack.push( root );
        tmpRes.push( Integer.toString( root.val ) );
        while( !stack.isEmpty() ) {
            TreeNode node = stack.pop();
            String tmpStr = tmpRes.pop();
            if( node.left == null && node.right == null ) {
                res.add( tmpStr );
                continue;
            }
            if( node.right != null ) {
                stack.push( node.right );
                tmpRes.push( tmpStr + "->" + node.right.val );
            }
            if( node.left != null ) {
                stack.push( node.left );
                tmpRes.push( tmpStr + "->" + node.left.val );
            }
        }
        return res;
    }


思考过程:

* 如果当前节点是叶子节点, 那么把累积的结果保存进结果集.
* 如果当前节点不是叶子节点, 那么考虑左右子树
* 如果左子树不为空, 累积结果并递归进入左子树
* 如果右子树不为空, 累积结果并递归进入右子树

根据以上的操作步骤直接写出递归代码或迭代代码.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(n)`