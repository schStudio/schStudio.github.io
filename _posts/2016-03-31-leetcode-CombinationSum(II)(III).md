---
layout: post
title: CombinationSum(II)(III)
category: Leetcode
---

* content
{:toc}

## CombinationSum

### 题目描述

> Given a set of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.
> 
> The same repeated number may be chosen from C unlimited number of times.
> 
> Note:
> 
> * All numbers (including target) will be positive integers.
> * Elements in a combination (a1, a2, … , ak) must be in non-descending order. (ie, > a1 ≤ a2 ≤ … ≤ ak).
> * The solution set must not contain duplicate combinations.
> 
> For example, given candidate set 2,3,6,7 and target 7,
> A solution set is: 
> 
> `[7]`
> 
> `[2, 2, 3]`

### 解法

代码如下:
		
    public static List<List<Integer>> combinationSum( 
            int[] candidates, int target ) {
        // 排序, 后面才可以进行剪枝
        Arrays.sort( candidates );
        List<List<Integer>> res = new ArrayList<>();
        helperCombinationSum2( 
                candidates, target, 0, new ArrayList<Integer>(), res );
        return res;
    }

    private static void helperCombinationSum2(
            int[] candidates, int target, int t, List<Integer> tmpRes, 
            List<List<Integer>> res) {
	    // 约束条件
        if( t == candidates.length ) {
            return;
        }
        // 剪枝
        if( candidates[t] > target ) {
            return;
        }
        
        // 遍历解空间
        int i = 0;
        for( ; i*candidates[t] <= target; ++i ) {
            if( i != 0 ) tmpRes.add( candidates[t] );
            if( i*candidates[t] == target ) {
                res.add( new ArrayList<Integer>( tmpRes) );
            } else {
                helperCombinationSum2( candidates, 
                	target-i*candidates[t], t+1, tmpRes, res );
            }
        }
        while( i-- > 1 )
            tmpRes.remove( tmpRes.size()-1 );
    }
        
思考过程: `回溯法`. 对于candidates数组中的每一个值, 可以无限重复取, 但是实际上只要取到一定程度就就可以结束了, 结束条件: i*candidates[t] > target, 也就是取[0...i]的情况, [i+1...∞]不取.

时空复杂度: 时间复杂度是`不确定`, 空间复杂度是O(n): 递归的空间, 排除构造出来的结果集

- - -

## CombinationSum II

### 题目描述

> Given a collection of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.
>
> Each number in C may only be used once in the combination.
>
> Note:
> 
> * All numbers (including target) will be positive integers.
> * Elements in a combination (a1, a2, … , ak) must be in non-descending order. (ie, a1 ≤ a2 ≤ … ≤ ak).
> * The solution set must not contain duplicate combinations.
> 
> For example, given candidate set 10,1,2,7,6,1,5 and target 8, 
> 
> A solution set is: 
>
> `[1, 7]`
>
> `[1, 2, 5]`
>
> `[2, 6]`
>
> `[1, 1, 6]`


### 解法

代码如下:
		
    public static List<List<Integer>> combinationSum2( 
			int[] candidates, int target ) {
		// 排序, 后面才可以进行剪枝
		Arrays.sort( candidates );
		List<List<Integer>> res = new ArrayList<>();
		helperCombinationSum( 
				candidates, target, 0, new ArrayList<Integer>(), res );
		return res;
	}

	private static void helperCombinationSum(
			int[] candidates, int target, int t, List<Integer> tmpRes, 
			List<List<Integer>> res) {
		if( t == candidates.length ) {
			return;
		}
		// 剪枝
		if( candidates[t] > target ) {
			return;
		}
		
		// 统计重复元素个数
		int repeatCount = 0;
		while( repeatCount+t < candidates.length && 
				candidates[t] == candidates[t+repeatCount])
			++repeatCount;
		
		// 遍历解空间
		int i = 0;
		for( ; i <= repeatCount && i*candidates[t] <= target; ++i ) {
			if( i != 0 ) tmpRes.add( candidates[t] );
			if( i*candidates[t] == target ) {
				res.add( new ArrayList<Integer>( tmpRes) );
			} else {
				helperCombinationSum( candidates, 
				    target-i*candidates[t], t+repeatCount, tmpRes, res );
			}
		}
		while( i-- > 1 )
			tmpRes.remove( tmpRes.size()-1 );
	}
	
        
思考过程: 同CombinationSum一样. 不一样的地方: CombinationSum中的每个元素取[0,∞]个, 这里每个元素取[0...R]个, R表示元素的重复个数, 所以代码里面就多了限制条件: `i <= repeatCount` 而且进入下一层要跨越重复元素: `t+repeatCount`

时空复杂度: 时间复杂度是`不确定`, 空间复杂度是O(n): 递归的空间, 排除构造出来的结果集

### 总结

> 借助已有的处理模型, 思考新问题中`影响因素`对结果产生的影响, 并且从原有模型中调整得到解决方案

- - -

## CombinationSum III

### 题目描述

> Find all possible combinations of k numbers that add up to a number n, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.
>
> Ensure that numbers within the set are sorted in ascending order.
>
>
> Example 1:
>
> Input: k = 3, n = 7
>
> Output:
>
> `[[1,2,4]]`
>
> Example 2:
>
> Input: k = 3, n = 9
>
> Output:
>
> `[[1,2,6], [1,3,5], [2,3,4]]`


### 解法

代码如下:
		
    public static List<List<Integer>> combinationSum3( int n, int k ) {
        List<List<Integer>> res = new ArrayList<>();
        helperCombinationSum3( n, k, new ArrayList<Integer>(), res );
        return res;
    }

    private static void helperCombinationSum3( int n, int k, List<Integer> tmpRes, 
            List<List<Integer>> res) {
        // 约束条件
        if( k <= 0 || n <= 0 ) {
            return;
        }
        
        // 保证结果子集中不可能出现重复元素
        int i = tmpRes.size()==0 ? 1 : tmpRes.get(tmpRes.size()-1)+1;
        for( ; i <= 9; ++i ) {
            tmpRes.add( i );
            if( n-i == 0 && k-1 == 0 ) {
                res.add( new ArrayList<Integer>(tmpRes) );
            } else {
                helperCombinationSum3( n-i, k-1, tmpRes, res );
            }
            tmpRes.remove( tmpRes.size()-1 );
        }
    }
        
思考过程: `回溯法`. 本质上跟上面两题都是一样的. 不同的地方就是: `约束条件`, `解空间的遍历方式`

时空复杂度: 时间复杂度是`不确定`, 空间复杂度是O(n): 递归的空间, 排除构造出来的结果集