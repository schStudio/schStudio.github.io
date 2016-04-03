---
layout: post
title: Unique Binary Search Trees(II)
category: Leetcode
---

* content
{:toc}

## Unique Binary Search Trees

### 题目描述

> Given n, how many structurally unique BST's (binary search trees) that store values 1...n?
>
> For example,
> 
> Given n = 3, there are a total of 5 unique BST's.
>
>
       1         3     3      2      1
        \       /     /      / \      \
         3     2     1      1   3      2
        /     /       \                 \
       2     1         2                 3
>

### 解法

代码如下:
		
    public static int numTrees( int n ) {
        if( n <= 1 ) return 1;
        // dp[i]: 有i个不同数字的numTrees
        int[] dp = new int[n+1];

        dp[0] = dp[1] = 1;

        for( int i = 2; i <= n; ++i ) {
            for( int j = 0; j < i; ++j )
                dp[i] += dp[j] * dp[i-1-j];
        }
        return dp[n];
    }
        
思考过程: `动态规划`. 我们的目的是求出有**n个不同元素时的树的个数**(定义为`dp[n]`). 首先我们考虑一个元素能够带来的影响. 假设这个元素是`i`, 那么元素`i`可以充当`root`, 也可以不充当`root`. 如果充当`root`, 那么其左子树个数有**`i-1`个不同元素**(`dp[i-1]`), 如果不充当`root`, 那么元素`i`会对**其他`root`为`[1...i-1]`的树**(定义为`f[k, i]`, `k∈[1...i-1]`)产生影响, 所以我们需要定义`dp[n]`和`f[i, n]`, 如下:

* `dp[n]`: 有`n`个不同元素时树的个数
* `f[i, n]`: 有`n`个不同元素时且root为`i`的树的个数

`dp[n]`实际上等于root为`[1...n]`的情况总和, 而`f[i, n]`实际上等于`左子树总数*右子树总数`, 所以我们可以容易得到以下关系式:

* `dp[n]` = `f[1, n]` + `f[2, n]` + `...` + `f[n, n]`
* `f[i, n]` = `dp[i-1]` * `dp[n-i]`

把`f[i, n]`代入`dp[n]`我们可以得到:

* `dp[n]` = `dp[0]*dp[n-1]` + `dp[1]*dp[n-2]` + `...` + `dp[n-i]*dp[0]`


时空复杂度: 时间复杂度是O(n^2), 空间复杂度是O(n)

- - -

## Unique Binary Search Trees II

### 题目描述

> Given n, generate all structurally unique BST's (binary search trees) that store values 1...n.
>
> For example,
> Given n = 3, your program should return all 5 unique BST's shown below.
>
        1         3     3      2      1
         \       /     /      / \      \
          3     2     1      1   3      2
         /     /       \                 \
        2     1         2                 3

### 解法

代码如下:
		
    public static List<TreeNode> generateTrees( int n ) {
        if( n <= 0 ) return new ArrayList<TreeNode>();
        return helperGenerateTrees( 1, n );
    }
    private static List<TreeNode> helperGenerateTrees( int left, int right ) {
        List<TreeNode> res = new ArrayList<>();
        if( left > right ) {
            res.add( null );
        } else {
            for( int i = left; i <= right; ++i ) {
                List<TreeNode> rootLeft = helperGenerateTrees( left, i-1 );
                List<TreeNode> rootRight = helperGenerateTrees( i+1, right );
                for( TreeNode nodeLeft : rootLeft )
                    for( TreeNode nodeRight : rootRight ) {
                        TreeNode node = new TreeNode( i );
                        node.left = nodeLeft;
                        node.right = nodeRight;
                        res.add( node );
                    }
            }
        }
        return res;
    }s
	
思考过程: 我们目标是生成所有的`Unique Binary Search Trees`. 一开始, 考虑一个元素对原始结果集会产生什么样的影响? 加入一个元素`n`, 原始结果集都需要考虑加入`n`的情况, 操作比较麻烦,无从下手. 

那么换个角度思考, 一个元素作为树根可以产生怎样的结果集? 假设在`[1...n]`中有一个元素`i`, 以它为树根可以产生结果集: `[1...i-1]的结果集 * [i+1...n]的结果集`, 那么我们尝试定义一个方法: 

	给定范围, 产生结果集

到这里发现我们的目标可以转化为`求[1...n]范围的结果集`, 问题也就这样递归地解决了.

时空复杂度: 时间复杂度是`目前不确定`, 空间复杂度是`目前不确定`