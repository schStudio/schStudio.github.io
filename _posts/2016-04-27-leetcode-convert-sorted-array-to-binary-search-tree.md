---
layout: post
title: Convert Sorted Array to Binary Search Tree
category: Leetcode
---

* content
{:toc}

## Convert Sorted Array to Binary Search Tree

### 题目描述

> Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

### 解法

代码如下:

    public static TreeNode sortedArrayToBST( int[] nums ) {
        return helper( nums, 0, nums.length - 1 );
    }
    private static TreeNode helper( int[] nums, int low, int high ) {
        if( low > high ) return null;
        if( low == high ) return new TreeNode( nums[low] );
        int mid = low + ( high - low ) / 2;
        TreeNode root = new TreeNode( nums[mid] );
        root.left = helper( nums, low, mid - 1 );
        root.right = helper( nums, mid + 1, high );
        return root;
    }

思考过程: 

构建一棵平衡二叉树, 必须保证左子树的高度与右子树的高度相差不超过1.

构建一棵平衡二叉查找树, 只要保证了左子树的个数与右子树的个数相差不超过1, 其左子树的高度与右子树的高度相差不会超过1, 也就保证二叉树是平衡的.

所以定义一个方法, 根据范围构建平衡二叉查找树. 每次找到范围的中间值作为树的根节点, 递归构建左边范围的二叉树作为左子树, 递归构建右边范围的二叉树作为右子树, 最后返回根节点.

时空复杂度: 时间复杂度是`O(nlogn)`, 空间复杂度是`O(logn)`