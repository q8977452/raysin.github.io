---
layout: post
title:  "August LeetCoding Challenge Week 2"
date:   2020-08-14 21:55:30 +0800
categories: Leetcode
tags : Leetcode, Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by August LeetCoding Challenge Week 2.




* Path Sum III
    ```python
    def pathSum(self, root: TreeNode, sum: int) -> int:
        if not root: 
            return 0
        return self.dfs(root, sum) + self.pathSum(root.left, sum) + self.pathSum(root.right, sum)
    
    def dfs(self, root, sum):
        res = 0
        if not root: 
            return res
        sum -= root.val
        if sum == 0:
            res += 1
        res += self.dfs(root.left, sum)
        res += self.dfs(root.right, sum)
        return res
    ```
    
* Rotting Oranges
    ```python
    def orangesRotting(self, grid: List[List[int]]) -> int:
        M, N = len(grid), len(grid[0])
        fresh = 0
        q = collections.deque()
        for i in range(M):
            for j in range(N):
                if grid[i][j] == 1:
                    fresh += 1
                elif grid[i][j] == 2:
                    q.append((i, j))
        if fresh == 0:
            return 0
        dirs = [(0, 1), (0, -1), (-1, 0), (1, 0)]
        step = 0
        while q:
            size = len(q)
            for i in range(size):
                x, y = q.popleft()
                for d in dirs:
                    nx, ny = x + d[0], y + d[1]
                    if nx < 0 or nx >= M or ny < 0 or ny >= N or grid[nx][ny] != 1:
                        continue
                    grid[nx][ny] = 2
                    q.append((nx, ny))
                    fresh -= 1
            step += 1
        if fresh != 0:
            return -1
        return step - 1
    ```
* Excel Sheet Column Number
    ```python
    def titleToNumber(self, s: str) -> int:
        return reduce(lambda x,y: x*26 + y, map(lambda x:ord(x)-64, s), 0)
    ```
    
    
* H-Index
    ```python
    def hIndex(self, citations: List[int]) -> int:
        N = len(citations)
        citations.sort()
        l, r = 0, N - 1
        H = 0
        while l <= r:
            mid = int(l + (r - l) / 2)
            H = max(H, min(citations[mid], N - mid))
            if citations[mid] < N - mid:
                l = mid + 1
            else:
                r = mid - 1
        return H
    ```
    
* Pascal's Triangle II
    ```python
    def getRow(self, rowIndex: int) -> List[int]:
        res = [1] * (rowIndex + 1)
        for i in range(2, rowIndex + 1):
            for j in range(i - 1, 0, -1):
                res[j] += res[j - 1]
        return res
    ```
    
* Iterator for Combination
    ```python
    class CombinationIterator:

        def __init__(self, characters: str, combinationLength: int):
            self.s = list(itertools.combinations(characters, combinationLength))
            self.index = 0

        def next(self) -> str:
            self.index += 1
            return "".join(self.s[self.index - 1])

        def hasNext(self) -> bool:
            return self.index < len(self.s)
    ```
    
* Longest Palindrome
    ```python
    def longestPalindrome(self, s: str) -> int:
        d={}
        res=0
        for i in s:
            d[i] = 1 if i not in d else d[i]+1
        for i in d:
            res+=d[i]-d[i]%2
        return res+1 if res<len(s) else res
    ```