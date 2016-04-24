---
layout: post
title: Verify Preorder Serialization of a Binary Tree
category: Leetcode
---

* content
{:toc}

## Verify Preorder Serialization of a Binary Tree

### 题目描述

> One way to serialize a binary tree is to use pre-order traversal. When we encounter a `non-null` node, we record the node's value. If it is a `null` node, we record using a sentinel value such as `#`.
> 
         _9_
            /   \
           3     2
          / \   / \
         4   1  #  6
        / \ / \   / \
        # # # #   # #
>
> For example, the above binary tree can be serialized to the string `"9,3,4,#,#,1,#,#,2,#,6,#,#"`, where `#` represents a `null` node.
> 
> Given a string of comma separated values, verify whether it is a correct preorder traversal serialization of a binary tree. Find an algorithm without reconstructing the tree.
> 
> Each comma separated value in the string must be either an integer or a character `'#'` representing `null` pointer.
> 
> You may assume that the input format is always valid, for example it could never contain two consecutive commas such as `"1,,3"`.
> 
> Example 1:
>
> `"9,3,4,#,#,1,#,#,2,#,6,#,#"`
>
> Return `true`
> 
> Example 2:
>
> `"1,#"`
>
> Return `false`
> 
> Example 3:
>
> `"9,#,#,1"`
>
> Return `false`


### 解法

代码如下:

    public static boolean isValidSerialization( String preorder ) {
        Stack<String> stack = new Stack<>();
        String[] strs = preorder.split(",");
        if( !"#".equals( strs[0] ) )
            stack.push( strs[0] );
        int i = 1;
        while( !stack.isEmpty() ) {
            stack.pop();
            if( i + 1 >= strs.length )
                return false;
            String left = strs[i];
            String right = strs[i+1];
            if( !"#".equals( right ) )
                stack.push( right );
            if( !"#".equals( left ) )
                stack.push( left );
            i += 2;
        }
        return i >= strs.length;
    }

思考过程:

给出二叉树的前序遍历, 判断其合法性. 那么首先应该考虑非法的情况有哪些:

* 非空节点缺少子节点
* 空节点拥有子节点

因为给出的序列化是前序, 所以使用栈来模拟前序遍历的过程.

对于第一种非法情况, 给出以下解释:

在模拟过程中, 如果缺少子节点, 直接返回`false`.

对于第二种非法情况, 给出以下解释:

栈中保存的是非空子节点, 如果空节点有子节点, 那么栈就会提前变空. 例如`"9,#,#,1"`, 模拟完了之后还有剩余节点`1`.

综上可以在模拟过程中判断是否缺少子节点, 在模拟退出后判断是否剩余子节点, 如果满足以上任意条件就返回`false`.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(n)`