---
layout: post
title: Invert Binary Tree
category: Leetcode
---

* content
{:toc}

## Invert Binary Tree

### 题目描述

> Invert a binary tree.
>
         4
       /   \
      2     7
     / \   / \
    1   3 6   9
>
> to
>
         4
       /   \
      7     2
     / \   / \
    9   6 3   1
>
> Trivia:
>
> This problem was inspired by this original tweet by Max Howell:
>
> Google: 90% of our engineers use the software you wrote (Homebrew), but you can’t invert a binary tree on a whiteboard so fuck off.
>
> MaxHowell写过这样一条推特 :
>
> 谷歌: 90%的工程师都使用你写的软件(Homebrew), 但你连二叉树的翻转都写不出来(白板编程), ~滚~

### 解法

代码如下:

    public TreeNode invertTree( TreeNode root ) {
        if( root == null ) return null;
        TreeNode left = invertTree( root.right );
        TreeNode right = invertTree( root.left );
        root.left = left;
        root.right = right;
        return root;
    }

思考过程:

如果树为空, 那么返回`null`, 否则分别翻转两棵子树, 然后再赋值, 最后返回根节点.

> 注意不要一边翻转一边赋值, 例如下面的代码是错误的:
>
    public TreeNode invertTree( TreeNode root ) {
        if( root == null ) return null;
        root.left = invertTree( root.right ); // root.left引用丢失
        root.right = invertTree( root.left );
        return root;
    }

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(h)`