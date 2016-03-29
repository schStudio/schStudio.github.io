---
layout: post
title: Subsets(II)
category: Leetcode
---

* content
{:toc}

## Subsets

### 题目描述

> Given a set of distinct integers, nums, return all possible subsets.
> 
> Note:
> 
> * Elements in a subset must be in non-descending order.
> * The solution set must not contain duplicate subsets.
> 
> For example,
> If nums = [1,2,3], a solution is:
    [
      [3],
      [1],
      [2],
      [1,2,3],
      [1,3],
      [2,3],
      [1,2],
      []
    ]

### 解法

* 递归方式

    代码如下:
		
        static List<List<Integer>> res = new ArrayList<>();
        
        public static List<List<Integer>> subsetsRecursion( int[] nums ) {
            Arrays.sort( nums );
            helperRecursion( nums, 0, new ArrayList<Integer>() );
            return res;
        }
        private static void helperRecursion( int[] nums, int t, List<Integer> tmpRes ) {
            // 获得一个子集
            if( t == nums.length ) {
                res.add( new ArrayList<Integer>(tmpRes) );
                return;
            }
            // 不选择当前深度的节点
            helperRecursion( nums, t+1, tmpRes );

            // 选择当前深度的节点
            tmpRes.add( nums[t] );
            helperRecursion( nums, t+1, tmpRes );
            tmpRes.remove( tmpRes.size()-1 );
        }
        
	思考过程: 求一个集合的所有子集, 这个问题可以归类为`回溯法`中的`子集树`问题, 而且这棵树是一棵完全二叉树, 利用`回溯法`中的递归写法可以很容易的写出代码.
    
    处理过程: 利用一个变量`t`表示当前树的深度, 在`t`深度的时候, 有两种选择, 一种是不选择当前深度的节点并且进入下一层, 另一种是选择当前深度的节点并且进入下一层. 其终止条件就是到达叶子节点.
    
    时空复杂度: 时间复杂度是O(2^n), 空间复杂度是O(n): 递归的空间, 排除构造出来的子集空间

* 迭代方式

	代码如下:
    
        public static List<List<Integer>> subsetIteration( int[] nums ) {
		    Arrays.sort( nums );
            List<List<Integer>> res = new ArrayList<>();
            
            // 初始状态: 结果集中包含了子集: 空集
            res.add( new ArrayList<Integer>() );
            
            // 构造过程: 动态生成每一个子集并且加入结果集
            for( int i = 0; i < nums.length; ++i ) {
                int subsetCount = res.size();
                for( int j = 0; j < subsetCount; ++j ) {
                    List<Integer> newSubset = new ArrayList<>( res.get(j) );
                    newSubset.add( nums[i] );
                    res.add( newSubset );
                }
            }
            
            // 结束状态: 返回结果集
            return res;
        }

	思考过程: 在思考问题解之前, 可以先思考另外一个问题: 对结果集产生影响的基本因素是什么? 基本因素就是数组中的元素. 再思考一个问题: 基本因素是如何影响结果集的? 当原本数组加入一个新的元素的时候, 由于元素在子集中可选可不选, 不选的话不会对原本的结果集产生影响, 选择的话原本的结果集会多添加一些新子集, 而且这些新子集=旧子集+新元素
    
    处理过程: 首先, 对于任何的数组, 其结果集中一定有一个空集; 之后对每一个元素, 产生一些新子集: 新子集=旧子集+新元素, 并且把新子集加入结果集中; 最后返回结果集.
    
    时空复杂度: 时间复杂度O(2^n), 空间复杂度O(1): 排除构造出来的子集空间

### 总结

> 对于有处理模型的问题, 我们很容易想到解决方案; 但是有时候我们并不知道处理模型是什么, 我们应该从小方面入手, 思考问题的基本因素, 从基本因素展开分析, 从而构造出一个问题的解决方案

- - -

## Subsets II

### 题目描述

> Given a collection of integers that might contain duplicates, nums, return all possible subsets.
>
> Note:
> 
> * Elements in a subset must be in non-descending order.
> * The solution set must not contain duplicate subsets.
> 
> For example,
> If nums = [1,2,2], a solution is:
    [
      [2],
      [1],
      [1,2,2],
      [2,2],
      [1,2],
      []
    ]

### 解法

	public static List<List<Integer>> subsetsIteration( int[] nums ) {
		Arrays.sort( nums );
		List<List<Integer>> res = new ArrayList<>();
        
		// 初始状态: 结果集中包含了子集: 空集
		res.add( new ArrayList<Integer>() );
        
		// 构造过程: 对每一个元素构造出新子集并加入结果集
		for( int i = 0; i < nums.length; ) {
			// 统计重复个数
			int repeatNum = 0;
			while( i+repeatNum < nums.length &&
					nums[i] == nums[repeatNum+i] ) ++repeatNum;
			// 构造子集
			int subsetCount = res.size();
			for( int j = 0; j < subsetCount; ++j ) {
				List<Integer> subset = new ArrayList<Integer>( res.get(j) );
				for( int k = 0; k < repeatNum; ++k ) {
					subset.add( nums[i] );
					res.add( new ArrayList<Integer>( subset ) );
				}
			}
			i += repeatNum;
		}
        
		// 结束状态: 返回结果集
		return res;
	}
    

思考过程: 其基本思考过程同上面的Subsets迭代方式差不多, 不同点在于出现了重复元素, 那么还是一样, 我们思考重复元素对原始结果集的影响是什么? 思考这样一个例子[ 1, 2, 2 ], 当处理完元素[1]的时候, 我们得到的结果集是{ {}, {1} }, 接下来对于重复元素[2], 我们可以构造出来的新子集=旧子集+(1或2)个[2], 也就是说, 对于重复元素, 我们可以选择1-n个重复元素加入旧子集形成新子集, 对于重复元素的解决方案就出来了, 最后可以得到上面的代码

### 总结

> 还是一样, 对于一个新的问题, 我们应该从基本的角度去思考, 从而展开到一个问题的解决方案