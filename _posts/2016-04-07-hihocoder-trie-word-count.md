---
layout: post
title: Trie树
category: hihoCoder
---

* content
{:toc}

## Trie树

### 题目描述

> 小Hi和小Ho是一对好朋友，出生在信息化社会的他们对编程产生了莫大的兴趣，他们约定好互相帮助，在编程的学习道路上一同前进。
>
> 这一天，他们遇到了一本词典，于是小Hi就向小Ho提出了那个经典的问题：“小Ho，你能不能对于每一个我给出的字符串，都在这个词典里面找到以这个字符串开头的所有单词呢？”
>
> 身经百战的小Ho答道：“怎么会不能呢！你每给我一个字符串，我就依次遍历词典里的所有单词，检查你给我的字符串是不是这个单词的前缀不就是了？”
>
> 小Hi笑道：“你啊，还是太年轻了！~假设这本词典里有10万个单词，我询问你一万次，你得要算到哪年哪月去？”
>
> 小Ho低头算了一算，看着那一堆堆的0，顿时感觉自己这辈子都要花在上面了...
>
> 小Hi看着小Ho的囧样，也是继续笑道：“让我来提高一下你的知识水平吧~你知道树这样一种数据结构么？”
>
> 小Ho想了想，说道：“知道~它是一种基础的数据结构，就像这里说的一样！”
>
> 小Hi满意的点了点头，说道：“那你知道我怎么样用一棵树来表示整个词典么？”
>
> 小Ho摇摇头表示自己不清楚。
>
> ![hihocoder-trie-dictionary]({{ site.url }}/assets/hihocoder/hihocoder-trie-dictionary.jpg)
>
> “你看，我们现在得到了这样一棵树，那么你看，如果我给你一个字符串ap，你要怎么找到所有以ap开头的单词呢？”小Hi又开始考校小Ho。
>
> **输入**
>
> 输入的第一行为一个正整数`n`，表示词典的大小，其后`n`行，每一行一个单词（不保证是英文单词，也有可能是火星文单词哦），单词由不超过10个的小写英文字母组成，可能存在相同的单词，此时应将其视作不同的单词。接下来的一行为一个正整数`m`，表示小Hi询问的次数，其后`m`行，每一行一个字符串，该字符串由不超过10个的小写英文字母组成，表示小Hi的一个询问。
>
> 在`20%`的数据中`n, m<=10`，词典的字母表大小<=2.
>
> 在`60%`的数据中`n, m<=1000`，词典的字母表大小<=5.
>
> 在`100%`的数据中`n, m<=100000`，词典的字母表大小<=26.
>
> **输出**
>
> 对于小Hi的每一个询问，输出一个整数Ans,表示词典中以小Hi给出的字符串为前缀的单词的个数。

### 解法

Trie树作为一种数据结构, 我们首先要先定义这个树的API和底层实现

* Trie树的API :
	* `insert( String word )` : 插入一个单词
	* `searchCount( String word )` : 查询以`word`开头的单词个数

定义了API之后, 我们继续考虑底层的数据结构该如何实现, 首先Trie树的基本单位是`节点`, 所以我们需要定义`节点`, `节点`有一个域表示字母, 还有不确定数量的`子节点`, 因此节点的定义如下:

* `TreeNode` :
	* `val` : `char`
	* `childs` : `List<TreeNode>`(也可以用哈希表等来实现)

定义出子节点之后, 那么一棵Trie树用一个根节点就可以表示

* Trie树的底层实现 :
	* `root` : `TreeNode`

最后实现我们之前定义的API

* `insert( String word )` : 插入一个单词, 第一步是找到一个位置, 也就是在Trie树中已经保存的最长前缀, 然后把剩下部分构造加入Trie树即可; 这是Trie树通用操作, 针对这道题, 还需要在插入的时候统计每个节点的count, 便于查询的时候返回. 所以在定义`TreeNode`的时候还要加入`count`
* `searchCount( String word )` : 根据给定的单词前缀, 不断查找前缀的每个字符, 一旦找到则进入下一个字符的查询, 最后如果每个字符都找到了, 直接返回节点的`count`, 如果没有找到每个字符, 说明查询的单词前缀不存在, 返回`0`.

代码如下:

    import java.util.*;

    public class Trie {
        private TreeNode root;

        public Trie() {
            root = new TreeNode();
        }

        // 插入单词
        public void insert(String str) {
            TreeNode currNode = root;
            int i;
            for (i = 0; i < str.length(); ++i) {
                char c = str.charAt(i);
                TreeNode child = currNode.child(c);
                if (child == null) {
                    break;
                } else {
                    currNode = child;
                    currNode.count++;
                }
            }
            for (int j = i; j < str.length(); ++j) {
                char c = str.charAt(j);
                TreeNode node = new TreeNode(c);
                currNode.addChild(node);
                currNode = node;
            }
        }

        // 查询单词前缀的单词个数
        public int searchCount(String str) {
            TreeNode currNode = root;
            int i;
            for (i = 0; i < str.length(); ++i) {
                char c = str.charAt(i);
                TreeNode child = currNode.child(c);
                if (child == null) {
                    break;
                } else {
                    currNode = child;
                }
            }
            return i == str.length() ? currNode.count : 0;
        }

        private class TreeNode {
            char val;
            int count = 1;
            List<TreeNode> childs = new ArrayList<TreeNode>();

            public TreeNode() { val = '$';}
            public TreeNode(char c) { val = c;}

            public TreeNode child(char c) {
                for (TreeNode node : childs) {
                    if (node.val == c) {
                        return node;
                    }
                }
                return null;
            }

            public void addChild(TreeNode child) {
                childs.add(child);
            }
        }
    }