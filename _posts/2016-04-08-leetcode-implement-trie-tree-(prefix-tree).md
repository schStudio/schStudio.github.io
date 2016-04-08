---
layout: post
title: Implement Trie (Prefix Tree)
category: Leetcode
---

* content
{:toc}

## Implement Trie (Prefix Tree)

### 题目描述

> Implement a trie with insert, search, and startsWith methods.
>
> Note:
> 
> You may assume that all inputs are consist of lowercase letters `a-z`.

### 解法

`TrieNode`代码如下:

    private static class TrieNode {
        char val;
        boolean isWord;
        List<TrieNode> childs = new ArrayList<>();

        public TrieNode( char val ) { this.val = val; }
        public TrieNode() { this.val = '$'; };

        public TrieNode getChild( char val ) {
            for( TrieNode child : childs )
                if( child.val == val ) return child;
            return null;
        }
        public void addChild( TrieNode child ) {
            childs.add( child );
        }
    }

`Trie`代码如下:

    static class Trie {
        private TrieNode root;

        public Trie() {
            root = new TrieNode();
        }

        // Inserts a word into the trie.
        public void insert(String word) {
            TrieNode currNode = root;
            int i;
            for( i = 0; i < word.length(); ++i ) {
                TrieNode childNode = currNode.getChild( word.charAt( i ) );
                if( childNode == null ) break;
                else currNode = childNode;
            }
            for( ; i < word.length(); ++i ) {
                TrieNode childNode = new TrieNode( word.charAt( i ) );
                currNode.addChild( childNode );
                currNode = childNode;
            }
            currNode.isWord = true;
        }

        // Returns if the word is in the trie.
        public boolean search(String word) {
            TrieNode currNode = root;
            int i;
            for( i = 0; i < word.length(); ++i ) {
                TrieNode childNode = currNode.getChild( word.charAt( i ) );
                if( childNode == null ) break;
                else currNode = childNode;
            }
            return i == word.length() ? currNode.isWord : false;
        }

        // Returns if there is any word in the trie
        // that starts with the given prefix.
        public boolean startsWith(String prefix) {
            TrieNode currNode = root;
            int i;
            for( i = 0; i < prefix.length(); ++i ) {
                TrieNode childNode = currNode.getChild( prefix.charAt( i ) );
                if( childNode == null ) break;
                else currNode = childNode;
            }
            return i == prefix.length() ? true : false;
        }
    }

思考过程: 可以参考[这里](http://schstudio.github.io/2016/04/07/hihocoder-trie-word-count/)
