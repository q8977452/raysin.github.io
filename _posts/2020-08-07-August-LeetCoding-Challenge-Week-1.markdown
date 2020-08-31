---
layout: post
title:  "August LeetCoding Challenge Week 1"
date:   2020-08-07 21:55:30 +0800
categories: Leetcode
tags : Leetcode Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by August LeetCoding Challenge Week 1.





* Detect Capital
    ```python
    def detectCapitalUse(self, word: str) -> bool:
        test = word.upper()
        test1 = word.lower()
        if test == word or test1 == word:
            return True
        for i in range(1,len(word)):
            if word[i] == test[i]:return False
        return True
    ```
    
* Logger Rate Limiter
    ```python
    class Logger:

        def __init__(self):
            """
            Initialize your data structure here.
            """
            self.data = dict()


        def shouldPrintMessage(self, timestamp: 'int', message: 'str') -> 'bool':
            """
            Returns true if the message should be printed in the given timestamp, otherwise returns false.
            If this method returns false, the message will not be printed.
            The timestamp is in seconds granularity.
            """
            if message not in self.data or timestamp - self.data[message] >= 10:
                self.data[message] = timestamp
                return True
            else:
                return False
    ```
    
* Design HashSet
    ```python
    class MyHashSet:

        def __init__(self):
            """
            Initialize your data structure here.
            """
            self.buckets = 1000
            self.itemsPerBucket = 1001
            self.table = [[] for _ in range(self.buckets)]

        def hash(self, key):
            return key % self.buckets

        def pos(self, key):
            return key // self.buckets

        def add(self, key: int) -> None:
            hashkey = self.hash(key)
            if not self.table[hashkey]:
                self.table[hashkey] = [0] * self.itemsPerBucket
            self.table[hashkey][self.pos(key)] = 1

        def remove(self, key: int) -> None:
            hashkey = self.hash(key)
            if self.table[hashkey]:
                self.table[hashkey][self.pos(key)] = 0

        def contains(self, key: int) -> bool:
            """
            Returns true if this set contains the specified element
            """
            hashkey = self.hash(key)
            return (self.table[hashkey] != []) and (self.table[hashkey][self.pos(key)] == 1)
    ```
    
* Valid Palindrome
    ```python
    def isPalindrome(self, s: str) -> bool:
        isValid = lambda x : x == x[::-1]
        string = ''.join([x for x in s.lower() if x.isalnum()])
        return isValid(string)
    ```
    
* Power of Four
    ```python
    def isPowerOfFour(self, num: int) -> bool:
        if num <= 0: return False
        while num % 4 == 0:
            num /= 4
        return num == 1
    ```
    
* Add and Search Word - Data structure design
    ```python
    class Node(object):
        def __init__(self):
            self.children = collections.defaultdict(Node)
            self.isword = False

    class WordDictionary:

        def __init__(self):
            """
            Initialize your data structure here.
            """
            self.root = Node()

        def addWord(self, word: str) -> None:
            """
            Adds a word into the data structure.
            """
            current = self.root
            for w in word:
                current = current.children[w]
            current.isword = True

        def search(self, word: str) -> bool:
            """
            Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter.
            """
            return self.match(word, 0, self.root)

        def match(self, word, index, root):
            if root == None:
                return False
            if index == len(word):
                return root.isword
            if word[index] != '.':
                return root != None and self.match(word, index + 1, root.children.get(word[index]))
            else:
                for child in root.children.values():
                    if self.match(word, index + 1, child):
                        return True
            return False
    ```
    
* Find All Duplicates in an Array
    ```python
    def findDuplicates(self, nums: List[int]) -> List[int]:
        dup=[]
        if len(nums)==0:
            return dup
        for i in range(len(nums)):
            nums[abs(nums[i])-1]=0-nums[abs(nums[i])-1]
            if nums[abs(nums[i])-1]>0:
                dup.append(abs(nums[i]))
        return dup
    ```
    
* Vertical Order Traversal of a Binary Tree
    ```python
    class Solution:
        def verticalTraversal(self, root: TreeNode) -> List[List[int]]:
            self.m_ = list()
            self.dfs(root, 0, 0)
            self.m_.sort()
            res = [[self.m_[0][2]]]
            for i in range(1, len(self.m_)):
                if self.m_[i][0] == self.m_[i - 1][0]:
                    res[-1].append(self.m_[i][2])
                else:
                    res.append([self.m_[i][2]])
            return res

        def dfs(self, root, x, y):
            if not root: return
            self.m_.append((x, -y, root.val))
            self.dfs(root.left, x - 1, y - 1)
            self.dfs(root.right, x + 1, y - 1)
    ```