---
layout: post
title: Pascal's Triangle(II)
category: Leetcode
---

* content
{:toc}

## Pascal's Triangle

### 题目描述

> Given numRows, generate the first numRows of Pascal's triangle.
>
> For example, given numRows = 5,
> 
> Return
> 
    [
         [1],
        [1,1],
       [1,2,1],
      [1,3,3,1],
     [1,4,6,4,1]
    ]

### 解法

代码如下:

    public static List<List<Integer>> generate( int numRows ) {
        List<List<Integer>> res = new ArrayList<>();
        if( numRows == 0 ) return res;

        List<Integer> curr = Arrays.asList( 1 );
        res.add( curr );
        for( int i = 1; i < numRows; ++i ) {
            List<Integer> next = new ArrayList<>();
            for( int j = -1; j < curr.size(); ++j ) {
                if( j+1 == 0 ) 
                    next.add( curr.get( j+1 ) );
                else if( j+1 == curr.size() )
                    next.add( curr.get( j ) );
                else
                    next.add( curr.get( j ) + curr.get( j+1 ) );
            }
            res.add( next );
            curr = next;
        }
        return res;
    }

思考过程: 模拟生成过程, 但是注意边界条件的判断.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(1)`

- - -

## Pascal's Triangle II

### 题目描述

> Given an index k, return the kth row of the Pascal's triangle.
>
> For example, given `k = 3`,
>
> Return `[1,3,3,1]`.
>
> Note:
>
> Could you optimize your algorithm to use only `O(k)` extra space?

### 解法

代码如下:

    public static List<Integer> getRow( int rowIndex ) {
        List<Integer> res = new ArrayList<>( rowIndex+1 );
        res.add( 1 );
        if( rowIndex == 0 ) return res;

        for( int i = 1; i <= rowIndex; ++i ) {
            for( int j = res.size()-1; j > 0; --j )
                res.set( j, res.get(j)+res.get(j-1) );
            res.add( 1 );
        }
        return res;
    }

思考过程: 下一行的结果是由上一行的结果生成的, 而且其规律如以下所示:

* `next[i] = 1 ( i == 0 )`
* `next[i] = curr[i] + curr[i-1] ( 0 < i < n )`
* `next[i] = 1 ( i == n )`

所以下一行可以在当前行进行修改 :

* 如果`i == 0`, `next[i] == curr[i] == 1`, 不做修改
* 如果`0 < i < n`, `next[i] == curr[i] + curr[i-1]`, 修改`curr[i]`
* 如果`i == n`, `next[n] == 1`, 添加`[1]`

> 注意在循环生成`next[i]`的时候要从右往左生成, 因为`next[i]`需要借助`curr[i-1]`元素; 如果从左往右生成, 那么`curr[i-1]`的值已经在上一次被修改过了, 如此就会产生错误的结果.

时空复杂度: 时间复杂度是`O(n^2)`, 空间复杂度是`O(1)`