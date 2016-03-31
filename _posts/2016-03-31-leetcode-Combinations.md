---
layout: post
title: Combinations
category: Leetcode
---

* content
{:toc}

## Combinations

### 题目描述

> Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.
> 
> For example,
> 
> If n = 4 and k = 2, a solution is:
> [
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]

### 解法

代码如下:
		
    public static List<List<Integer>> combine( int n, int k ) {
        List<List<Integer>> res = new ArrayList<>();
        helperCombine( n, 1, k, new ArrayList<Integer>(), res );
        return res;
    }
    public static void helperCombine( 
            int n, int t, int k, List<Integer> tmpRes, List<List<Integer>> res ) {
        // 约束条件
        if( t > n ) {
            return;
        }
        // 不选取当前值
        helperCombine( n, t+1, k, tmpRes, res );
        
        // 选取当前值
        tmpRes.add( t );
        if( k-1 == 0 ) {    // 获得一个结果
            res.add( new ArrayList<Integer>(tmpRes) );
        } else {            // 进入下一层
            helperCombine( n, t+1, k-1, tmpRes, res );
        }
        tmpRes.remove( tmpRes.size()-1 );
    }
        
思考过程: `回溯法`. 对于[1...n]中的每一个元素, 可取可不取

时空复杂度: 时间复杂度是O(2^n), 空间复杂度是O(n): 递归的空间, 排除构造出来的结果集

### 总结

> 把`约束条件`放在方法头部更容易理解
