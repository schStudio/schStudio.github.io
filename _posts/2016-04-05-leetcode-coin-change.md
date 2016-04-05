---
layout: post
title: Coin Change
category: Leetcode
---

* content
{:toc}

## Coin Change

### 题目描述

> You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.
>
> Example 1:
> `coins = [1, 2, 5]`, `amount = 11`
> return 3 `(11 = 5 + 5 + 1)`
>
> Example 2:
> `coins = [2]`, `amount = 3`
> return -1.
>
> Note:
> You may assume that you have an infinite number of each kind of coin.

### 解法

代码如下:

    public static int coinChange( int[] coins, int amount ) {
        // dp[i] : 价格为i时找回的最少硬币个数
        int[] dp = new int[amount+1];
        for( int i = 1; i <= amount; ++i ) {
            // 一开始假设需要无限个硬币
            dp[i] = Integer.MAX_VALUE;
            // 找最少硬币
            for( int j = 0; j < coins.length; ++j ) {
                if( i - coins[j] == 0 ) {
                    dp[i] = 1;
                } else if( i - coins[j] > 0 && dp[i-coins[j]] != -1 ) {
                    dp[i] = Math.min( dp[i], dp[i-coins[j]]+1 );
                }
            }
            // 如果还是需要无限个硬币, 说明没有结果
            dp[i] = dp[i]==Integer.MAX_VALUE ? -1 : dp[i];
        }
        return dp[amount];
    }

思考过程: `动态规划`. 考虑一个`amount`:

* 如果`amount`可以用一个硬币找回, 那么结果为1
* 如果`amount`不能用一个硬币找回, 那么`amount-coins[i]`需要的最少硬币个数`+1`就是结果

> 注意我们是要找到最小值, 所以一开始查找之前先定义为最大值

时空复杂度: 时间复杂度是`O(n*k)`, 空间复杂度是`O(n)`

> `n`表示`amount`, `k`表示硬币个数