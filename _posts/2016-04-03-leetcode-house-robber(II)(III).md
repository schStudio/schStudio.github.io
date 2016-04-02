---
layout: post
title: House Robber(II)(III)
category: Leetcode
---

* content
{:toc}

## House Robber

### 题目描述

> You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.
>
> Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

### 解法

代码如下:
		
    public static int rob( int[] nums ) {
    	// maxBroken : 前一次抢劫状态的最大值
        // maxUnbroken : 前一次非抢劫状态的最大值
        int maxBroken = 0, maxUnbroken = 0;
        for( int i = 0; i < nums.length; ++i ) {
            int t = maxBroken;
            maxBroken = Math.max( maxUnbroken+nums[i], maxBroken );
            maxUnbroken = Math.max( maxUnbroken, t );
        }
        return Math.max( maxBroken, maxUnbroken );
    }
        
思考过程: `动态规划`. `当前最大值`有2种情况: 

1. `抢劫当前家庭后的最大值` = max( `前一次非抢劫状态最大值`+`当前值`, `前一次抢劫状态最大值` )
2. `不抢劫当前家庭的最大值` = max( `前一次非抢劫状态最大值`, `前一次抢劫状态最大值` )

* 最优子结构: `当前最大值`依赖于`前一次最大值`
* 重叠子问题: 求`1.`和`2.`的时候有一个重叠的子问题`前一次抢劫状态最大值`

时空复杂度: 时间复杂度是O(n), 空间复杂度是O(1)

- - -

## House Robber II

### 题目描述

> Note: This is an extension of House Robber.
>
> After robbing those houses on that street, the thief has found himself a new place for his thievery so that he will not get too much attention. This time, all houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, the security system for these houses remain the same as for those in the previous street.
>
> Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

### 解法

代码如下:
		
    public static int rob( int[] nums ) {
        if( nums.length == 0 ) return 0;
        // 抢劫第一户家庭
        int max1 = nums[0] + robHelper( nums, 2, nums.length-1 );
        // 不抢劫第一户家庭
        int max2 = robHelper( nums, 1, nums.length );
        return Math.max( max1, max2 );
    }
    
    public static int robHelper( int[] nums, int left, int right ) {
        // maxBroken : 前一次抢劫状态的最大值
        // maxUnbroken : 前一次非抢劫状态的最大值
        int maxBroken = 0, maxUnbroken = 0;
        for( int i = left; i < right; ++i ) {
            int t = maxBroken;
            maxBroken = Math.max( maxUnbroken+nums[i], maxBroken );
            maxUnbroken = Math.max( maxUnbroken, t );
        }
        return Math.max( maxBroken, maxUnbroken );
    }
	
思考过程: 同`House Robber`相同, 在此基础上多了环的问题, 那么就需要考虑环对原本解决方案带来的影响: 环的问题归于头尾之间有影响关系, 那么我们可以分情况考虑: `抢劫第一户家庭`和`不抢劫第一户家庭`

1. `抢劫第一户家庭`的话, 我们就肯定不能抢劫第二户家庭和最后一户家庭, 所以抢劫第[3...n-1]户家庭
2. `不抢劫第一户家庭`的话, 我们可以抢劫第[2...n]户家庭

最后只要考虑这2个情况的最大值即可

时空复杂度: 时间复杂度是(n), 空间复杂度是(1)

- - -

## House Robber III

### 题目描述

> The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.
>
> Determine the maximum amount of money the thief can rob tonight without alerting the police.
>
> Example 1:
>
>           3
         / \
        2   3
         \   \ 
          3   1
>
> Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
> 
> Example 2:
>
>           3
         / \
        4   5
       / \   \ 
      1   3   1
>
> Maximum amount of money the thief can rob = 4 + 5 = 9.

### 解法

代码如下:
		
    public static int rob( TreeNode root ) {
        return robHelper( root )[0];
    }
    
    public static int[] robHelper( TreeNode root ) {
        if( root == null ) return new int[]{ 0, 0, 0 };
        
        int[] robLeft = robHelper( root.left );
        int[] robRight = robHelper( root.right );
        
        // robRoot: 
        //        robRoot[0]: 抢劫最大值
        //        robRoot[1]: 左子树抢劫最大值
        //        robRoot[2]: 右子树抢劫最大值
        int[] robRoot = new int[3];
        robRoot[0] = Math.max( robLeft[0]+robRight[0], 
                robLeft[1]+robLeft[2]+robRight[1]+robRight[2]+root.val );
        robRoot[1] = robLeft[0];
        robRoot[2] = robRight[0];
        
        return robRoot;
    }
	
思考过程: 一样是`动态规划`, 而且其特征更加明显:

* 最优子结构: `当前树最大值` =  max( `左子树最大值`+`右子树最大值`, `左左子树最大值`+`左右子树最大值`+`右左子树最大值`+`右右子树最大值`+`当前值` )
* 重叠子问题: 求解`左子树最大值`的时候需要子问题`左左子树最大值`+`左右子树最大值`

> 注:
> 
> * 不抢当前节点: `左子树最大值`+`右子树最大值`
> * 抢劫当前节点: `左左子树最大值`+`左右子树最大值`+`右左子树最大值`+`右右子树最大值`+`当前值`

时空复杂度: 时间复杂度是(2^n), 空间复杂度是(n)