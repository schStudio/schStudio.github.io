---
layout: post
title: Construct Binary Tree from Preorder(PostOrder) and Inorder Traversal
category: Leetcode
---

* content
{:toc}

## Construct Binary Tree from Preorder and Inorder Traversal

### 题目描述

> Given preorder and inorder traversal of a tree, construct the binary tree.

### 解法

代码如下:

    public static TreeNode buildTree( int[] preorder, int[] inorder ) {
        return helperBuildTree( preorder, 0, preorder.length-1,
                                inorder, 0, inorder.length-1 );
    }

    private static TreeNode helperBuildTree( 
            int[] preorder, int preleft, int preright,
            int[] inorder, int inleft, int inright ) {
        if( preleft > preright ) return null;

        TreeNode root = new TreeNode( preorder[preleft] );

	    // 计算左子树的节点数
        int len = 0;
        for( int i = inleft; i <= inright; ++i )
            if( inorder[i] == root.val ) {
                len = i - inleft;
                break;
            }
	    // 构造左子树
        root.left = helperBuildTree( preorder, preleft+1, preleft+len,
                                     inorder, inleft, inleft+len-1 );
	    // 构造右子树
        root.right = helperBuildTree( preorder, preleft+1+len, preright,
                                      inorder, inleft+len+1, inright );

        return root;
    }

思考过程: `递归`. 首先要明白树的这3种遍历的实际意义:

* 前序遍历preorer : `节点本身` => `左子树` => `右子树`
* 中序遍历inorder : `左子树` => `节点本身` => `右子树`
* 后序遍历postorder : `左子树` => `右子树` => `节点本身`

知道了这个访问顺序之后, 就可以比较容易看出该如何递归地构建该二叉树了.

`前序+中序`构建二叉树过程 : 前序遍历中的第一个值就是当前的根节点, 构建该根节点, 之后找到根节点在中序遍历中的位置, 找到位置就可以统计出左子树的节点数, 同样右子树也确定了, 所以递归构造左右子树, 最后返回当前根节点.


时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(n)`

> 注: `n`表示树的节点个数

- - -

## Construct Binary Tree from Postorder and Inorder Traversal

### 题目描述

> Given inorder and postorder traversal of a tree, construct the binary tree.

### 解法

代码如下:

    public static TreeNode buildTree( int[] postorder, int[] inorder ) {
        return helperBuildTree( postorder, 0, postorder.length-1,
                                inorder, 0, inorder.length-1 );
    }

    private static TreeNode helperBuildTree( 
            int[] postorder, int postleft, int postright,
            int[] inorder, int inleft, int inright ) {
        if( postleft > postright ) return null;

        TreeNode root = new TreeNode( postorder[postright] );

        // 计算左子树的节点数
        int len = 0;
        for( int i = inleft; i <= inright; ++i )
            if( inorder[i] == root.val ) {
                len = i - inleft;
                break;
            }
        // 构造左子树
        root.left = helperBuildTree( postorder, postleft, postleft+len-1,
                                     inorder, inleft, inleft+len-1 );
        // 构造右子树
        root.right = helperBuildTree( postorder, postleft+len, postright-1,
                                      inorder, inleft+len+1, inright );

        return root;
    }

思考过程: `递归`. 与上一题同样的算法思想.

`后序+中序`构建二叉树 : 后序遍历中的最后一个值就是当前的根节点, 构建给节点, 之后找到根节点在中序遍历中的位置, 找到位置就可以统计出左子树的节点数, 同样右子树也确定了, 所以递归构造左右子树, 最后返回当前根节点.


时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(n)`