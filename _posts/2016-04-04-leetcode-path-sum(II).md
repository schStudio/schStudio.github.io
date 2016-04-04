---
layout: post
title: Path Sum
category: Leetcode
---

* content
{:toc}

## Path Sum

### 题目描述

> Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.
>
> For example:
> 
> Given the below binary tree and sum = 22,
> 
               5
              / \
             4   8
            /   / \
           11  13  4
          /  \      \
         7    2      1
>
> return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.

### 解法

代码如下:
		
    public static boolean hasPathSum( TreeNode root, int sum ) {
        if( root == null ) return false;
        if( root.val == sum && 
                root.left == null && 
                root.right == null ) return true;
        return hasPathSum( root.left, sum-root.val ) || 
                hasPathSum( root.right, sum-root.val);
    }
        
思考过程: `DFS`. **有关树的问题一般都是以递归的思想解决的, 因为树的定义本身就是递归的.**

时空复杂度: 时间复杂度是O(n): n表示树的所有节点数, 空间复杂度是O(h): h表示树的高度

- - -

## Path Sum II

### 题目描述

> Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.
>
> For example:
> Given the below binary tree and sum = 22,
>
               5
              / \
             4   8
            /   / \
           11  13  4
          /  \    / \
         7    2  5   1
>
> return
> 
### 解法

代码如下:

    public static List<List<Integer>> PathSum( TreeNode root, int sum ) {
        List<List<Integer>> res = new ArrayList<>();
        helperPathSum( root, sum, new ArrayList<Integer>(), res );
        return res;
    }

    private static void helperPathSum( TreeNode root, int sum,
            List<Integer> tmpRes, List<List<Integer>> res ) {
        if( root == null ) return;

        if( sum == root.val && root.left == null && root.right == null ) {
            tmpRes.add( root.val );
            res.add( new ArrayList<Integer>(tmpRes) );
            tmpRes.remove( tmpRes.size()-1 );
            return;
        }

        tmpRes.add( root.val );
        helperPathSum( root.left, sum-root.val, tmpRes, res );
        helperPathSum( root.right, sum-root.val, tmpRes, res );
        tmpRes.remove( tmpRes.size()-1 );
    }

思考过程: `DFS`. 相比于`Path Sum`, 区别就是要记录所有的路径, 那么只需要多添加一些参数就可以了: `tmpRes`用于记录当前路径, `res`表示结果路径集.

时空复杂度: 时间复杂度是O(n): n表示树的所有节点数, 空间复杂度是O(h): h表示树的高度

### 总结

> 本题提醒了我: 在包含有递归的算法中, 一定要保证进入递归前的条件状态一致. 例如在调用`helperPathSum`的时候, `root`的值是未选取的, 那么递归里面就需要去选取处理, 并且保证进入下一轮递归之前条件状态一致.