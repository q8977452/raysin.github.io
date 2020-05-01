---
layout: post
title:  "30-Day LeetCoding Challenge Week4"
date:   2020-04-30 22:44:30 +0800
categories: Leetcode
tags : Leetcode Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by 30-Day LeetCoding Challenge Week4.




* Subarray Sum Equals K
    * https://www.youtube.com/watch?v=mKXIH9GnhgU
    ```python
    def subarraySum(self, nums: List[int], k: int) -> int:
        n = len(nums)
        d = collections.defaultdict(int)
        d[0] = 1
        sum, res = 0, 0
        for i in range(n):
            sum += nums[i]
            if sum - k in d:
                res += d[sum-k]
            d[sum] +=1
        return res
    ```
* Bitwise AND of Numbers Range
    * https://www.cnblogs.com/grandyang/p/4431646.html
    ```python
    def rangeBitwiseAnd(self, m: int, n: int) -> int:
        # Record translation times
        i = 0
        
        while m != n:
            m >>= 1
            n >>= 1
            i += 1
        return m << i
    ```
* LRU Cache
    * https://blog.csdn.net/qq_17550379/article/details/85780337
    ```python
    class LRUCache:

        def __init__(self, capacity: int):
            self.capacity = capacity
            self.stack = list()
            self.cache = dict()

        def get(self, key: int) -> int:
            if key in self.cache:
                self.stack.remove(key)
                self.stack.append(key)
                return self.cache[key]

            return -1

        def put(self, key: int, value: int) -> None:
            if key in self.cache:
                self.stack.remove(key)
            else:
                if len(self.cache) == self.capacity:
                    del self.cache[self.stack[0]]
                    self.stack.pop(0)

            self.stack.append(key)
            self.cache[key] = value
    ```
    * [如何使用 Python 實現 LRU Cache 快取置換機制](https://blog.techbridge.cc/2019/04/06/how-to-use-python-implement-least-recently-used/)
        * 常見的快取文件置換機制有：
            1. FIFO（First In, First Out）
                * 先進先出算法
            2. LFU（Least Frequently Used）
                * 最近最不常使用算法
            3. LRU（Least Recently Used）
                * 最近最少使用算法
            4. NMRU（Not Most Recently Used）
                * 非最近使用算法
        * 評量快取機制的指標主要有兩種：
            * 延遲（latency）
                * 指的是命中後回傳對應資料的所花的時間
            * 命中率（hit rate）
                * 是需要的資料在快取中被找到的頻率
    
* Jump Game
    * https://blog.csdn.net/yangjingjing9/article/details/76209758
    ```python
    def canJump(self, nums: List[int]) -> bool:
        step = nums[0]
        for i in range(1, len(nums)):
            if step > 0 :
                step -= 1
                step = max(step, nums[i])
            else:
                return False
        return True
    ```
* Longest Common Subsequence
    * https://www.geeksforgeeks.org/python-program-for-longest-common-subsequence/
        * A Naive recursive
        ```python
        def longestCommonSubsequence(X, Y, m, n): 
  
            if m == 0 or n == 0: 
               return 0; 
            elif X[m-1] == Y[n-1]: 
               return 1 + longestCommonSubsequence(X, Y, m-1, n-1)
            else: 
               return max(longestCommonSubsequence(X, Y, m, n-1),
               longestCommonSubsequence(X, Y, m-1, n))
        ```
        * Dynamic Programming 
        ```python
        def longestCommonSubsequence(self, text1: str, text2: str) -> int:
            m = len(text1)
            n = len(text2)

            L = [[None]*(n+1) for i in range(m+1)]

            for i in range(m + 1):
                for j in range(n + 1):
                    if i == 0 or j == 0:
                        L[i][j] = 0
                    elif text1[i-1] == text2[j-1]:
                        L[i][j] = L[i-1][j-1]+1
                    else:
                        L[i][j] = max(L[i-1][j],L[i][j-1])

            return L[m][n]
        ```
* Maximal Square
    * https://blog.csdn.net/fuxuemingzhu/article/details/82992233
    ```python
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        if not matrix: 
            return 0
        row = len(matrix)
        col = len(matrix[0])
        dp = [[0] * col for _ in range(row)]
        for i in range(row):
            dp[i][0] = int(matrix[i][0])
        for j in range(col):
            dp[0][j] = int(matrix[0][j])
        for i in range(1, row):
            for j in range(1, col):
                if int(matrix[i][j]) == 1:
                    dp[i][j] = min(dp[i][j - 1], dp[i - 1][j], dp[i - 1][j - 1]) + 1
        return max(map(max, dp)) ** 2
    ```
* First Unique Number
	* https://www.youtube.com/watch?v=JlDd3n6GP3w 
    ```python
    class FirstUnique:

        def __init__(self, nums: List[int]):
            self.list = []
            self.dict = {}
            for i in nums:
                self.add(i)

        def showFirstUnique(self) -> int:
            while len(self.list) > 0 and self.dict[self.list[0]] > 1:
                self.list.pop(0)
            if len(self.list) == 0:
                return -1
            else:
                return self.list[0]

        def add(self, value: int) -> None:
            if value in self.dict:
                self.dict[value] += 1
            else:
                self.dict[value] = 1
                self.list.append(value)
    ```