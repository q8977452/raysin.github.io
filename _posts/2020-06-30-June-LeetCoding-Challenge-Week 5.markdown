---
layout: post
title:  "June LeetCoding Challenge Week 5"
date:   2020-06-30 19:56:30 +0800
categories: Leetcode
tags : Leetcode Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by June LeetCoding Challenge Week 5.



* Unique Paths
    ```python
    def uniquePaths(self, m: int, n: int) -> int:
        total = m + n - 2
        v = n - 1
        def permutation(m, n):
            son = 1
            for i in range(m, m - n, -1):
                son *= i
            mom = 1
            for i in range(n, 0, -1):
                mom *= i
            return son / mom
        return int(permutation(total, min(v, total -v)))
    ```

* Word Search II
    * https://blog.csdn.net/qq_31494411/article/details/52884470
        ```python
        class TrieNode(object):
            def __init__(self):
                """
                Initialize your data structure here.
                """
                self.childs = {}
                self.isWord = False


        class Trie(object):
            def __init__(self):
                self.root = TrieNode()

            def insert(self, word):
                """
                Inserts a word into the trie.
                :type word: str
                :rtype: void
                """
                useroot = self.root
                for i in range(len(word)):
                    if word[i] in useroot.childs:
                        useroot = useroot.childs[word[i]]
                    else:
                        useroot.childs[word[i]] = TrieNode()
                        useroot = useroot.childs[word[i]]

                useroot.isWord = True

            def delete(self, word):
                useroot = self.root
                stack = []
                for i in range(len(word)):
                    stack.append([useroot, word[i]])
                    useroot = useroot.childs[word[i]]
                    if useroot is None:
                        return False
                if not useroot.isWord:
                    return False
                elif useroot.childs:
                    useroot.isWord = False
                    return True
                else:
                    while stack:
                        node, string = stack.pop()
                        del node.childs[string]
                        if len(node.childs) or node.isWord:
                            break

                    return True


        class Solution(object):
            def findWords(self, board, words):
                """
                :type board: List[List[str]]
                :type words: List[str]
                :rtype: List[str]
                """
                ans = []
                tree = Trie()
                visited = [[False] * len(board[0]) for _ in range(len(board))]
                for i in range(len(words)):
                    tree.insert(words[i])

                def dfs(x, y, root, word):
                    root = root.childs.get(board[x][y])
                    if not root:
                        return
                    visited[x][y] = True
                    for dx, dy in zip((1, 0, -1, 0), (0, 1, 0, -1)):
                        nx = x + dx
                        ny = y + dy
                        if nx < 0 or ny < 0 or nx > len(board) - 1 or ny > len(board[0]) - 1 or visited[nx][ny]:
                            continue
                        dfs(nx, ny, root, word + board[nx][ny])
                    if root.isWord:
                        ans.append(word)
                        tree.delete(word)
                    visited[x][y] = False

                for i in range(len(board)):
                    for j in range(len(board[0])):
                        dfs(i, j, tree.root, board[i][j])
                return ans
        ```