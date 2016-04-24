---
layout: post
title: Palindrome Pairs
category: Leetcode
---

* content
{:toc}

## Palindrome Pairs

### 题目描述

> Given a list of unique words. Find all pairs of distinct indices `(i, j)` in the given list, so that the concatenation of the two words, i.e. `words[i]` + `words[j]` is a palindrome.
>
> Example 1:
>
> Given `words = ["bat", "tab", "cat"]`
>
> Return `[[0, 1], [1, 0]]`
>
> The palindromes are `["battab", "tabbat"]`
>
> Example 2:
>
> Given `words = ["abcd", "dcba", "lls", "s", "sssll"]`
>
> Return `[[0, 1], [1, 0], [3, 2], [2, 4]]`
>
> The palindromes are `["dcbaabcd", "abcddcba", "slls", "llssssll"]`


### 解法

代码如下:

    static class TrieNode {
        TrieNode[] childs;
        int index;
        List<Integer> list;

        TrieNode() {
            childs = new TrieNode[26];
            index = -1;
            list = new ArrayList<>();
        }
    }

    public static List<List<Integer>> palindromePairs( String[] words ) {
        List<List<Integer>> res = new ArrayList<>();
        TrieNode root = new TrieNode();
        for( int i = 0; i < words.length; ++i )
            addWord( root, words, i );
        for( int i = 0; i < words.length; ++i )
            search( words, i, root, res );
        return res;
    }
    // Trie树查找合拼回文串
    private static void search(String[] words, int i, TrieNode root, List<List<Integer>> res) {
        // 考虑合并串较长的情况
        for( int j = 0; j < words[i].length(); ++j ) {
            if( root.index != -1 && 
	            root.index != i && 
	            isPalindrome( words[i], j, words[i].length()-1 ) )
                res.add( Arrays.asList( i, root.index ) );
            // 进入子节点
            root = root.childs[words[i].charAt( j )-'a'];
            if( root == null ) return;
        }
	    // 考虑合并串和被合并串等长的情况
        // 考虑被合并串较长的情况
        for( int index : root.list ) {
            if( i == index ) continue;
            res.add( Arrays.asList( i, index ) );
        }
    }
    // Trie树添加单词
    private static void addWord(TrieNode root, String[] words, int i) {
        for( int j = words[i].length() - 1; j >= 0; --j ) {
            // 记录非后缀部分是回文串的字符串
            if( isPalindrome( words[i], 0, j ) )
                root.list.add( i );
            // 进入下一个节点
            char c = words[i].charAt( j );
            if( root.childs[c-'a'] == null ) {
                root.childs[c-'a'] = new TrieNode();
            }
            root = root.childs[c-'a'];
        }
        root.list.add( i );
        root.index = i;
    }
    // 判断是否回文串
    private static boolean isPalindrome(String string, int i, int j) {
        while( i < j )
            if( string.charAt( i++ ) != string.charAt( j-- ) )
                return false;
        return true;
    }

思考过程: 算法参考自[这里](https://leetcode.com/discuss/91429/solution-with-structure-total-number-words-average-length)

暴力法解决方案: 合拼任意两个字符串并判断合拼后字符串是否为回文串, 时间复杂度为`O(n^2*k)`. `n`代表字符串个数, `k`代表字符串平均长度.

使用暴力法明显会导致超时, 因此需要继续思考另外的解决方案, 从回文串的判断入手, 因为回文串的性质是对称, 因此可以消除一些不必要的检查, 例如字符串`abc`和`cbd`, 由于字符`a`和字符`d`不对称, 所以就可以不必合并判断.

因此可以构建合适的数据结构来解决此问题, 这里使用Trie树, 也就是字典树, 进行字符串的查询.

例如字符串`abb`, 在Trie树中查询以`bba`后缀的字符串. 但是并不是一定要完全后缀, 例如`a`, `ba`,`bba`, `cbba`, `ccbba`等这些字符串都是满足条件的, 对这些例子进行分类可以得到如下三类:

假设`s1 = abb`表示合并串, `s2`代表被合并串

1. 合并串`s1`字符串长度比被合并串`s2`的长
2. 合并串`s1`字符串长度与被合并串`s2`的相等
3. 合并串`s1`字符串长度比被合并串`s2`的短

对于第1种情况, 如果`s2`的后缀表示和`s1`的部分前缀完全相同, 考虑`s1`中非前缀部分是否为回文串, 如果是则满足条件. 例如`s1 = abb`, `s2 = a`.

对于第2种情况, 如果`s2`的后缀表示和`s1`的前缀表示完全相同, 则满足条件

对于第3种情况, 如果`s2`的部分后缀表示和`s1`的前缀表示完全相同, 考虑`s2`中非后缀部分是否为回文串, 如果是则满足条件. 例如`s1 = abb`, `s2 = ccbba`.

结合以上3种情况进行设计Trie树数据结构和其操作方法.

这里假设合并串为查询串, 那么需要把所有串的后缀表达保存进Trie树, 然后进行查询操作.

查询过程中, 需要知道当前后缀表达是否为被合并串, 因此使用一个变量`index`表示被合并串的下标, `-1`表示该后缀表达不是被合并串.

对于情况1和情况2, 我们可以在查询过程中进行处理, 对于情况3则需要查询+遍历来解决. 再深入思考会发现, 如果我们在插入字符串后缀表达的时候, 顺便保存那些非后缀部分是回文串的字符串, 那么情况3就不需要遍历查询了.

因此可以给出Trie树数据结构如下:

    static class TrieNode {
        TrieNode[] childs;
        int index;
        List<Integer> list;

        TrieNode() {
            childs = new TrieNode[26];
            index = -1;
            list = new ArrayList<>();
        }
    }

下面给出插入操作:

    // Trie树插入单词
    private static void addWord(TrieNode root, String[] words, int i) {
        for( int j = words[i].length() - 1; j >= 0; --j ) {
            // 记录非后缀部分是回文串的字符串
            if( isPalindrome( words[i], 0, j ) )
                root.list.add( i );
            // 进入下一个节点
            char c = words[i].charAt( j );
            if( root.childs[c-'a'] == null ) {
                root.childs[c-'a'] = new TrieNode();
            }
            root = root.childs[c-'a'];
        }
        root.list.add( i );
        root.index = i;
    }

再给出查询操作:

    // Trie树查找合并回文串
    private static void search(String[] words, int i, TrieNode root, List<List<Integer>> res) {
        // 考虑合并串较长的情况
        for( int j = 0; j < words[i].length(); ++j ) {
            if( root.index != -1 && 
	            root.index != i && 
	            isPalindrome( words[i], j, words[i].length()-1 ) )
                res.add( Arrays.asList( i, root.index ) );
            // 进入子节点
            root = root.childs[words[i].charAt( j )-'a'];
            if( root == null ) return;
        }
        // 考虑合并串和被合并串等长的情况
        // 考虑被合并串较长的情况
        for( int index : root.list ) {
            if( i == index ) continue;
            res.add( Arrays.asList( i, index ) );
        }
    }

时空复杂度: 时间复杂度是`O(n*k^2)`, 空间复杂度是`O(1)`