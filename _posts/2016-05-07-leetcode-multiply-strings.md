---
layout: post
title: Multiply Strings
category: Leetcode
---

* content
{:toc}

## Multiply Strings

### 题目描述

> Given two numbers represented as strings, return multiplication of the numbers as a string.
> 
> Note:
> 
> The numbers can be arbitrarily large and are non-negative.
> 
> Converting the input string to integer is NOT allowed.
> 
> You should NOT use internal library such as BigInteger.

### 解法

代码如下:

    public static String multiply(String num1, String num2) {
        String sum = "0";
        String postFixZero = "";
        for(int i = 0; i < num2.length(); i++) {
            String val1 = mul(num1, num2.charAt(num2.length() - 1 - i));
            val1 += "0".equals(val1) ? "" : postFixZero;
            sum = add(sum, val1);
            postFixZero += "0";
        }
        return sum;
    }

    private static String add(String val1, String val2) {
        StringBuilder sb = new StringBuilder();
        int carry = 0;
        for(int i = 0; i < val1.length() || i < val2.length(); i++) {
            char c1 = val1.length() - 1 - i >= 0 ? val1.charAt(val1.length() - 1 - i) : '0';
            char c2 = val2.length() - 1 - i >= 0 ? val2.charAt(val2.length() - 1 - i) : '0';
            int sum = (c1 - '0') + (c2 - '0') + carry;
            sb.append(sum % 10);
            carry = sum / 10;
        }
        if(carry != 0)
            sb.append(carry);
        return sb.reverse().toString();
    }

    private static String mul(String num1, char val1) {
        if("0".equals(num1) || val1 == '0')
            return "0";
        StringBuilder sb = new StringBuilder();
        int carry = 0;
        for(int i = 0; i < num1.length(); i++) {
            char val2 = num1.charAt(num1.length() - 1 - i);
            int m = (val1 - '0') * (val2 - '0') + carry;
            sb.append(m % 10);
            carry = m / 10;
        }
        if(carry != 0)
            sb.append(carry);
        return sb.reverse().toString();
    }

思考过程: `模拟法`。

模拟乘法运算法则：

1. 从低位到高位，把乘数中的每一位都乘以被乘数，得到临时结果
2. 把所有的临时结果相加

因此，定义两个方法：

* `add` : 两个字符串相加
* `mul` : 字符乘以字符串

最后根据运算法则调用对应的方法执行即可。

时空复杂度: 时间复杂度是`O(n^2)`, 空间复杂度是`O(n^2)`