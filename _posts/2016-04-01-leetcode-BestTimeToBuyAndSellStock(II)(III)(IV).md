---
layout: post
title: BestTimeToBuyAndSellStock(II)(III)(IV)
category: Leetcode
---

* content
{:toc}

## BestTimeToBuyAndSellStock

### 题目描述

> Say you have an array for which the ith element is the price of a given stock on day i.
>
> If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

### 解法

* 暴力法

	代码如下:
    
        public static int maxProfitBruteForce( int[] prices ) {
            int maxProfit = 0;
            for( int i = 0; i < prices.length-1; ++i ) {
                for( int j = i+1; j < prices.length; ++j ) {
                    if( prices[j] - prices[i] > maxProfit )
                        maxProfit = prices[j] - prices[i];
                }
            }
            return maxProfit;
        }
    
    思考过程: 对于每一天, 我们都计算出这一天卖出可以获得的maxProfit
    
    时空复杂度: 时间复杂度是O(n^2), 空间复杂度是O(1)

* 分治法

	代码如下:
    
        public static int maxProfitDivideAndConquer( 
                int[] prices, int left, int right ) {
            if( left >= right ) return 0;
            int mid = left + (right-left)/2;
            int leftMaxProfit = maxProfitDivideAndConquer( prices, left, mid );
            int rightMaxProfit = maxProfitDivideAndConquer( prices, mid+1, right );
			// 求出左半部分最低价
            int leftMin = mid;
            for( int i = mid; i >= left; --i )
                if( prices[i] < prices[leftMin] )
                    leftMin = i;

			// 求出右半部分最高价
            int rightMax = mid;
            for( int i = mid; i <= right; ++i )
                if( prices[i] > prices[rightMax] )
                    rightMax = i;

            return Math.max( 
                    Math.max( leftMaxProfit, rightMaxProfit ), 
                    prices[rightMax]-prices[leftMin] );
        }
    
    思考过程: 对于给定的一段时间`[left...right]`, 我们从中间mid把时间段切割为`[left...mid]`和`[mid+1...right]`, 那么在`[left...right]`这段时间的解为以下的情况之一:
    
    * [left...mid] 的maxProfit
    * [mid+1...right] 的maxProfit
    * 右半部分最高价 - 左半部分最低价
    
	时空复杂度: 时间复杂度是O(nlogn), 空间复杂度是O(logn)

* 动态规划

	代码如下:
    
        public static int maxProfitDP( int[] prices ) {
            int leftMin = 0, maxProfit = 0;
            for( int i = 1; i < prices.length; ++i ) {
                if( prices[i] - prices[leftMin] > maxProfit )
                    maxProfit = prices[i] - prices[leftMin];
                if( prices[i] < prices[leftMin] )
                    leftMin = i;
            }
            return maxProfit;
        }
    
    思考过程: 由前面的暴力法可知, 我们对每一天都求出这一天卖出的maxProfit, 而这个maxProfit实际上是`当天价格-历史价格最低价`. 暴力法中我们遍历查询历史价格, 其实我们可以用一个变量来记录历史价格最低价, 也就是代码中的`leftMin`. 这样可以把查询时间从O(n)降到O(1).
    
    这种解法看似与DP没什么关系, 只是在原来的基础上做出了优化而已, 其实这个优化就是动态规划的思想了. 其体现如下:
    
    * 最优子结构: maxProfit[i] = max( maxProrit[i-1], prices[i]-prices[leftMin] )
    * 重叠子问题: leftMin的查找
    
	时空复杂度: 时间复杂度是O(n), 空间复杂度是O(1)

- - -

## BestTimeToBuyAndSellStock II

### 题目描述

> Say you have an array for which the ith element is the price of a given stock on day i.
>
> Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

### 解法


代码如下:
    
    public static int maxProfit( int[] prices ) {
        // S表示前一次卖的日期, B表示前一次买的日期
        int S = 0, B = 0, maxProfit = 0;
        for( int i = 1; i < prices.length; ++i ) {
            if( B >= S ) {        // 最近一次交易是买
                if( prices[i] < prices[B] ) {
                    B = i;
                } else if( prices[i] > prices[B] ) {
                    maxProfit += prices[i] - prices[B];
                    S = i;
                } 
            } else {            // 最近一次交易是卖
                if( prices[i] > prices[S] ) {
                    maxProfit += prices[i] - prices[S];
                    S = i;
                } else if( prices[i] < prices[S] ) {
                    B = i;
                }
            }
        }
        return maxProfit;
    }
    
思考过程: 因为交易次数无限, 所以只要赚钱就卖出去, 低价就买回来 -- `贪心算法`

我们使用两个变量: `S`表示前一次卖出的日期, `B`表示前一次买进的日期. 对于每一天, 先判断最近一次交易是买还是卖. 

* 最近一次交易是买, 如果今天价格更低就当作今天才买进, 如果今天价格更高就卖出去.
* 最近一次交易是卖, 如果今天价格更高就当作今天才卖出, 如果今天价格更低就买进来.
    
时空复杂度: 时间复杂度是O(n), 空间复杂度是O(1)

- - -

## BestTimeToBuyAndSellStock IV

### 题目描述

> Say you have an array for which the ith element is the price of a given stock on day i.
>
> Design an algorithm to find the maximum profit. You may complete at most k transactions.
>
> Note:
>
> You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

### 解法

代码如下:
    
    public static int maxProfitDP( int k, int[] prices ) {
        int len = prices.length;

        // 如果交易次数超过最大交易次数, 获取最大利润
        if( k > len/2 ) return quickSolve( prices );

        // dp[i][j]: 在第j天之前交易i次可以获得的最大利润
        int[][] dp = new int[k+1][len];
        for( int i = 1; i <= k; ++i ) {
            // holds: 买进状态下, 目前持有的最大利润
            int holds = -prices[0];
            for( int j = 1; j < len; ++j ) {
                dp[i][j] = Math.max( dp[i][j-1], holds+prices[j] );
                holds = Math.max( holds, dp[i-1][j-1]-prices[j] );
            }
        }
        return dp[k][len-1];
    }

    private static int quickSolve( int[] prices ) {
        int profit = 0;
        for( int i = 1; i < prices.length; ++i )
            if( prices[i] > prices[i-1] )
                profit += prices[i] - prices[i-1];
        return profit;
    }
    
思考过程: 我们的目的是找到n天之内进行最多k次交易的最大利润, 那么我们给出以下定义:

* `dp[i][j]`: j天之内进行最多i次交易的最大利润

我们发现dp[i][j]可能与价格prices[j]有关, 也可能无关.

* 如果无关, 容易知道`dp[i][j]`等于`dp[i][j-1]` (最优子结构性质)
* 如果有关, `prices[j]`是如何影响`dp[i][j]`的取值的呢? 如果`prices[j]`能够影响`dp[i][j]`, 那么就只能是`prices[j]`这个价格卖出获得更大的利润. 所以就需要记录一个变量`holds: 买进状态下, 持有的最大利润`

分析到这里就会发现

	dp[i][j] = max( dp[i][j-1], holds+prices[j] )

那么`holds`是如何受`prices[j]`的影响的呢? 因为holds表示买进状态, 所以如果`prices[j]`能够影响`holds`, 那么就只能是买进`prices[j]`, 而且买进之前的最大利润是`dp[i-1][j-1]`, 所以

	holds = max( holds, dp[i-1][j-1]-prices[j] )
    
时空复杂度: 时间复杂度是O(kn), 空间复杂度是O(kn)

- - -

## BestTimeToBuyAndSellStock III

### 题目描述

> Say you have an array for which the ith element is the price of a given stock on day i.
>
> Design an algorithm to find the maximum profit. You may complete at most two transactions.
>
> Note:
>
> You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

### 解法

代码如下:
    
    public static int maxProfitDP( int[] prices ) {
        int len = prices.length;
        int k = 2;
        // 如果交易次数超过最大交易次数, 获取最大利润
        if( k > len/2 ) return quickSolve( prices );

        // dp[i][j]: 在第j天之前交易i次可以获得的最大利润
        int[][] dp = new int[k+1][len];
        for( int i = 1; i <= k; ++i ) {
            // holds: 买进状态下, 目前持有的最大利润
            int holds = -prices[0];
            for( int j = 1; j < len; ++j ) {
                dp[i][j] = Math.max( dp[i][j-1], holds+prices[j] );
                holds = Math.max( holds, dp[i-1][j-1]-prices[j] );
            }
        }
        return dp[k][len-1];
    }

    private static int quickSolve( int[] prices ) {
        int profit = 0;
        for( int i = 1; i < prices.length; ++i )
            if( prices[i] > prices[i-1] )
                profit += prices[i] - prices[i-1];
        return profit;
    }
    
思考过程: 这是上一题中k=2的特殊情况
    
时空复杂度: 时间复杂度是O(n), 空间复杂度是O(n)