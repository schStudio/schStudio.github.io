---
layout: post
title: Binary Search Traversal
category: Leetcode
---

* content
{:toc}

## Binary Search Inorder Traversal

### 题目描述

> Given a binary tree, return the inorder traversal of its nodes' values.
>
> For example:
>
> Given binary tree `{1,#,2,3}`,
>
    1
     \
      2
     /
    3
>
> return `[1,3,2]`.
>
> Note: 
>
> * Recursive solution is trivial, could you do it iteratively?

### 解法

代码如下:

    public static List<Integer> inorderTraversal( TreeNode root ) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> s = new Stack<>();
        while( root != null ) {
            s.push( root );
            root = root.left;
        }
        while( !s.isEmpty() ) {
            TreeNode node = s.pop();
            res.add( node.val );
            node = node.right;
            while( node != null ) {
                s.push( node );
                node = node.left;
            }
        }
        return res;
    }

思考过程: 中序遍历的过程是`左`->`中`->`右`, 而且右边的节点可以由中间的节点获得, 所以处理过程是把`中`压在栈底, 然后往左走, 直到不能往左走为止; 之后开始进入遍历环节, 每次取出一个节点访问, 取出来的节点就是`中`, 访问了`中`就可以访问`右`了, 因为`右`也是一棵树, 所以同样压栈底往左走.

- - -

## Binary Search Preorder Traversal

### 题目描述

> Given a binary tree, return the preorder traversal of its nodes' values.
>
> For example:
>
> Given binary tree `{1,#,2,3}`,
>
    1
     \
      2
     /
    3
>
> return `[1,2,3]`.
>
> Note: 
>
> * Recursive solution is trivial, could you do it iteratively?

### 解法

代码如下:

    public static List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> s = new Stack<>();
        if( root == null ) return res;
        res.add( root.val );
        if( root.right != null ) s.push( root.right );
        if( root.left != null ) s.push( root.left );
        while( !s.isEmpty() ) {
            TreeNode node = s.pop();
            res.add( node.val );
            if( node.right != null ) s.push( node.right );
            if( node.left != null ) s.push( node.left );
        }
        return res;
    }

思考过程: 前序遍历的过程是`中`->`左`->`右`, 那么应该先访问`中`, 然后把`右`压入栈底, 再把`左`压入栈底, 重复此过程直到栈为空即可.

- - -

## Binary Search Postorder Traversal

### 题目描述

> Given a binary tree, return the postorder traversal of its nodes' values.
>
> For example:
>
> Given binary tree `{1,#,2,3}`,
>
    1
     \
      2
     /
    3
>
> return `[3,2,1]`.
>
> Note: 
>
> * Recursive solution is trivial, could you do it iteratively?

### 解法

代码如下:

    public static List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Stack<TreeNode> s = new Stack<>();
        if( root == null ) return res;
        res.add( root.val );
        if( root.left != null ) s.push( root.left );
        if( root.right != null ) s.push( root.right );
        while( !s.isEmpty() ) {
            TreeNode node = s.pop();
            res.add( node.val );
            if( node.left != null ) s.push( node.left );
            if( node.right != null ) s.push( node.right );
        }
        Collections.reverse( res );
        return res;
    }

思考过程: 后序遍历的过程是`左`->`右`->`中`, 由于对`中`的访问是放在最后面, 比较难以处理, 但是发现前序遍历与后序遍历的顺序是有旋转关系的, 如下:

* 后序遍历 : `左`->`右` I `中`
* 前序遍历 : `中` I `左`->`右`

假设`A'`表示`A`的翻转, 那么后序遍历可以由以下式子得到:

* (`中`->(`左`->`右`)')' => 
* (`中`->`右`->`左`)' => 
* `左`->`右` -> `中`

因此, 可以借助前序遍历的代码, 把`左`->`右`的访问顺序翻转下, 也就是`右`->`左`, 最后对结果进行翻转就可以了.