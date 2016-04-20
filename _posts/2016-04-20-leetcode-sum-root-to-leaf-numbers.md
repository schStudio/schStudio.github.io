---
layout: post
title: Sum Root to Leaf Numbers
category: Leetcode
---

* content
{:toc}

## Sum Root to Leaf Numbers

### 题目描述

> Given a binary tree containing digits from `0-9` only, each root-to-leaf path could represent a number.
>
> An example is the root-to-leaf path `1->2->3` which represents the number `123`.
>
> Find the total sum of all root-to-leaf numbers.
>
> For example,
>
        1
       / \
      2   3
>
The root-to-leaf path `1->2` represents the number `12`.
The root-to-leaf path `1->3` represents the number `13`.
>
> Return the `sum = 12 + 13 = 25`.

### 解法

* 递归版

    代码如下:

        private static int res = 0;

        public static int sumNumbers( TreeNode root ) {
            if( root == null ) return 0;
            helper( root );
            return res;
        }
        private static void helper( TreeNode root ) {
            if( root.left == null && root.right == null ) 
                res += root.val;
            if( root.left != null ) {
                root.left.val += root.val * 10;
                helper( root.left );
            }
            if( root.right != null ) {
                root.right.val += root.val * 10;
                helper( root.right );
            }
        }

* 迭代版

    代码如下:

        public static int sumNumbersIterative( TreeNode root ) {
            if( root == null ) return 0;
            int res = 0;
            Stack<TreeNode> stack = new Stack<>();
            stack.push( root );
            while( !stack.isEmpty() ) {
                TreeNode node = stack.pop();
                if( node.left == null && node.right == null )
                    res += node.val;
                if( node.left != null ) {
                    node.left.val += node.val * 10;
                    stack.push( node.left );
                }
                if( node.right != null ) {
                    node.right.val += node.val * 10;
                    stack.push( node.right );
                }
            }
            return res;
        }

思考过程: 

求一棵二叉树从根节点到叶子节点所有路径的数和, 如果二叉树为空, 返回0, 如果不为空, 统计二叉树的数和.

因此定义一个求二叉树数和的方法 :

* 如果二叉树本身就是叶子节点, 累加到结果
* 如果二叉树左子树不为空, 累加到左子树
* 如果二叉树右子树不为空, 累加到右子树
* 统计左子树的数和
* 统计右子树的数和

根据上面的条件就可以写出递归版和迭代版的代码.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`