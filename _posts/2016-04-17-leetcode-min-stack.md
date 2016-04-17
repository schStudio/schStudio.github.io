---
layout: post
title: Min Stack
category: Leetcode
---

* content
{:toc}

## Min Stack

### 题目描述

> Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
>
    push(x) -- Push element x onto stack.
    pop() -- Removes the element on top of the stack.
    top() -- Get the top element.
    getMin() -- Retrieve the minimum element in the stack.

### 解法

代码如下:

    public class MinStack {

        private Stack<Integer> stack = new Stack<>();
        private Stack<Integer> minStack = new Stack<>();

        public void push(int x) {
            stack.push( x );
            if( minStack.isEmpty() || x <= getMin() )
                minStack.push( x );
        }

        public void pop() {
            int x = stack.pop();
            if( x == minStack.peek() )
                minStack.pop();
        }

        public int top() {
            return stack.peek();
        }

        public int getMin() {
            return minStack.peek();
        }
    }

思考过程:

考虑使用一个变量来保存栈的最小值, 但是如果栈的最小值被弹出去了, 那么就需要扫描剩下的元素, 找到最小值, 这样时间复杂度就需要`O(n)`, 不满足要求.

前面考虑到弹出最小值之后要在剩下的元素中扫描新的最小值, 那么我们可以定义一个最小栈来保存每个位置以下最小值, 每次弹出就弹出两个栈的元素, 每次入栈就入栈两个栈, 但是最小栈就可能是新元素或者旧的最小值.

经过上面的考虑又发现最小栈实际上保存了很多冗余的最小值, 例如`push(1, 2, 3, 4, 5)`之后最小栈就是`[1, 1, 1, 1, 1]`, 我们可以进一步压缩最小栈空间. 如果新入栈元素比当前最小值还小, 我们才需要入栈最小栈; 最小值出栈的时候, 最小栈也同时出栈.

但是会有问题, 例如`push(1, 2, 3, 4, 1)`, 此时最小栈为`[1]`, 再执行`pop(1)`, 此时最小栈为`[]`, 这样就有问题了, 所以就算新元素等于最小值也应该入栈最小栈. 其根本原因在于我们是根据值的大小来出栈最小栈的, 所以就有可能出栈了别的最小值.

时空复杂度: 时间复杂度是`O(n)`, 空间复杂度是`O(n)`