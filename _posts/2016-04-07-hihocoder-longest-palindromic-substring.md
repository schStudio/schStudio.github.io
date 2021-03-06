---
layout: post
title: 最长回文子串
category: hihoCoder
---

* content
{:toc}

## 最长回文子串

### 题目描述

> 小Hi和小Ho是一对好朋友，出生在信息化社会的他们对编程产生了莫大的兴趣，他们约定好互相帮助，在编程的学习道路上一同前进。
>
> 这一天，他们遇到了一连串的字符串，于是小Hi就向小Ho提出了那个经典的问题：“小Ho，你能不能分别在这些字符串中找到它们每一个的最长回文子串呢？”
>
> 小Ho奇怪的问道：“什么叫做最长回文子串呢？”
>
> 小Hi回答道：“一个字符串中连续的一段就是这个字符串的子串，而回文串指的是`12421`这种从前往后读和从后往前读一模一样的字符串，所以最长回文子串的意思就是这个字符串中最长的身为回文串的子串啦~”
>
> 小Ho道：“原来如此！那么我该怎么得到这些字符串呢？我又应该怎么告诉你我所计算出的最长回文子串呢？
>
> 小Hi笑着说道：“这个很容易啦，你只需要写一个程序，先从标准输入读取一个整数N（N<=30)，代表我给你的字符串的个数，然后接下来的就是我要给你的那N个字符串（字符串长度<=10^6)啦。而你要告诉我你的答案的话，只要将你计算出的最长回文子串的长度按照我给你的顺序依次输出到标准输出就可以了！你看这就是一个例子。”
> 
> 样例输入
>
    3
    abababa
    aaaabaa
    acacdas

> 样例输出
> 
    7
    5
    3

### 解法1: 暴力法

题目要求求出最长回文子串, 有三个关键字`最长`/`回文`/`子串`

* 最长 : 定义一个变量记录
* 回文 : 定义一个判断方法
* 子串 : 找出所有子串

这样我们就可以把问题的解决方案描述出来了:

**对于每一个子串, 判断是否回文串, 如果是回文串, 再判断该子串的长度是否大于最长记录, 如果大于最长记录, 更新最长记录为该子串长度**

有了解决方案, 那么就把定义实现出来:

* 判断回文串方法 : 回文串的性质是对称, 由两边往中间扫描, 每次判断两边字符是否相等, 一旦不等, 则可断定为非回文串
* 子串 : 一个字符串可以拆分为两部分: 第一部分是第一个字符`[1]`, 第二部分是剩下的字符`[2...n]`. 对于第一部分而言, 可以组成`{ [1,2], [1,2,3], ..., [1...n] }`这些子串, 然后就可以排除`[1]`, 再对剩下的字符`[2...n]`以同样的算法处理

实现定义之后就可以进行编码

> 是否有必要对每个子串都进行判断呢? 
>
> 例如 : `[3...5]不是回文串` => `[2...6]也不是回文串` => `[1...7]也不是回文串`

### 解法2: 暴力法优化(1)

由前面的引申部分发现其实我们作出了多余的判断, 那么应该进行优化, 优化思路如下: 暴力法过程中对很多子串做出重复判断的原因在于**对同一个中点往两边扩展的串进行无差别的检查**, 实际上**同一个中点, 一旦扩展出来的串不满足回文串, 那么继续扩展也没有意义**

因此, 我们的考虑方向为字符串的每一个`中点`, 算法改进如下:

**对于字符串中的每一个字符, 不断往两边扩展, 一旦发现不满足回文串性质, 先判断长度是否比当前结果大, 如果是则更新当前结果, 然后进入下一个字符**

> 借助回文串的对称性质, 是否还有更多的信息来帮助优化算法吗?

### 解法3: 暴力法优化(2)

考虑如下情况:

![hihocoder-longest-palindromic-substring2]({{ site.url }}/assets/hihocoder/hihocoder-longest-palindromic-substring2.png)

说明 :

{% raw %}

* A<sub>2j-i</sub>与A<sub>i</sub>之间是关于A<sub>j</sub>的两个对称点
* 蓝色方框是A<sub>j</sub>的最长回文串
* 斜线方框是A<sub>2j-i</sub>与A<sub>i</sub>的最长回文串
* 有效长度是表示借助A<sub>j</sub>可以得到的A<sub>i</sub>回文串长度(至少)

{% endraw %}

根据回文串的对称性质, 我们可以发现:

![hihocoder-longest-palindromic-substring1]({{ site.url }}/assets/hihocoder/hihocoder-longest-palindromic-substring1.png)

由于`图1`的情况并不总是会出现的, 所以需要考虑所有的情况, 如下图:

![hihocoder-longest-palindromic-substring]({{ site.url }}/assets/hihocoder/hihocoder-longest-palindromic-substring.png)

首先给出定义:

`f(i)` : 中点为`i`的最长回文串长度

* 界内情况 : `f(i) >= f(2*j-i)`
* 边界情况 : `f(i) >= 2*有效长度 = f(2*j-i)`
* 越界情况 : `f(i) >= 2*有效长度`
* 无界情况 : `f(i) >= 1`

综上四种情况, 可以很容易得出:

* `f(i) = min( f(2*j-i), 2*有效长度 )`, `f(j)`能够包含中点`i`
* `f(i) = 1`, `f(j)`不能包含中点`i`

代码如下:

    public static int longestPalindromicSubstring( String str ) {

        // 转换
        StringBuilder sb = new StringBuilder();
        sb.append('#');
        for( int i = 0; i < str.length(); ++i )
            sb.append( str.charAt(i) ).append( '#' );
        str = sb.toString();

        // 结果
        int res = 0;

        // 右边界最长的中点j
        int j = 0;

        // len[i]: 中点i的最长回文串长度
        int[] len = new int[str.length()];
        len[0] = 1;

        for( int i = 1; i < str.length(); ++i ) {
            // 根据中点j得到len[i]的回文串长度(至少)
            if( j+len[j]/2 <= i ) 
                len[i] = 1;
            else
                len[i] = Math.min( len[2*j-i], len[j]-2*(i-j) );

            // 基于len[i]继续找最长回文串
            int start = i-len[i]/2, end = i+len[i]/2;
            while( start >= 0 && end < str.length() &&
                    str.charAt(start) == str.charAt(end) ) {
                --start;
                ++end;
            }
            len[i] = end-start-1;

            // 比较结果
            if( len[i] > res )
                res = len[i];
            // 更新中点j
            j = i+len[i]/2 > j+len[j]/2 ? i : j;
        }
        return res / 2;
    }

> 注:
>
> * 以上算法的出发点是以中点向两边扩展的情况, 但是有可能回文串是不以中点扩展的(如`1221`), 所以在一开始需要把字符串转化(例如`#1#2#2#1#`), 使得奇偶串都能够处理