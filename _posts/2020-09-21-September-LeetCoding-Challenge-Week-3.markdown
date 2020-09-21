---
layout: post
title:  "September LeetCoding Challenge Week 3"
date:   2020-09-21 21:18:30 +0800
categories: Leetcode
tags : Leetcode, Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by September LeetCoding Challenge Week 3.




* Length of Last Word
    ```python
    def lengthOfLastWord(self, s: str) -> int:
        return len(s.rstrip().split(' ')[-1])
    ```
    
* Maximum XOR of Two Numbers in an Array
    * Time limited
        ```python
        def findMaximumXOR(self, nums: List[int]) -> int:
            maxXor = 0

            n = len(nums)
            # Calculating xor of each pair 
            for i in range(n): 
                for j in range(i + 1, n): 
                    maxXor = max(maxXor, nums[i] ^ nums[j])

            return maxXor
        ```
    * Accepted
        ```python
        def findMaximumXOR(self, nums: List[int]) -> int:
            ans = mask = 0
            for x in range(32)[::-1]:
                mask += 1 << x
                prefixSet = set([n & mask for n in nums])
                temp = ans | 1 << x
                for prefix in prefixSet:
                    if temp ^ prefix in prefixSet:
                        ans = temp
                        break
            return ans
        ```
        
* Robot Bounded In Circle
    ```python
    def isRobotBounded(self, instructions: str) -> bool:
        dirs = [(0, 1), (-1, 0), (0, -1), (1, 0)]
        x, y = 0, 0
        curd = 0
        for i in instructions:
            if i == "G":
                x += dirs[curd][0]
                y += dirs[curd % 4][1]
            elif i == "L":
                curd = (curd + 1) % 4
            elif i == "R":
                curd = (curd - 1) % 4
        return (x == 0 and y == 0) or curd != 0
    ```
    
* Best Time to Buy and Sell Stock
    ```python
    def maxProfit(self, prices: List[int]) -> int:
        if len(prices) == 0:
            return 0
        minPrice = prices[0]
        maxProfit = 0
        for p in prices:
            if p < minPrice:
                minPrice = p
            elif p - minPrice > maxProfit:
                maxProfit = p - minPrice
        return maxProfit
    ```
    
* Sequential Digits
    ```python
    def sequentialDigits(self, low: int, high: int) -> List[int]:
        res = []
        for n in range(2,10):
            for i in range(1,10-n+1):
                s = int(''.join([str(i+j) for j in range(n)]))
                if low<=s<=high:
                    res.append(s)
        return res
    ```
    
* Unique Paths III
    ```python
    def uniquePathsIII(self, grid: List[List[int]]) -> int:
        R, C = len(grid), len(grid[0])

        def neighbors(r, c):
            for nr, nc in ((r-1, c), (r, c-1), (r+1, c), (r, c+1)):
                if 0 <= nr < R and 0 <= nc < C and grid[nr][nc] % 2 == 0:
                    yield nr, nc

        todo = 0
        for r, row in enumerate(grid):
            for c, val in enumerate(row):
                if val != -1: todo += 1
                if val == 1: sr, sc = r, c
                if val == 2: tr, tc = r, c

        self.ans = 0
        def dfs(r, c, todo):
            todo -= 1
            if todo < 0: return
            if r == tr and c == tc:
                if todo == 0:
                    self.ans += 1
                return

            grid[r][c] = -1
            for nr, nc in neighbors(r, c):
                dfs(nr, nc, todo)
            grid[r][c] = 0

        dfs(sr, sc, todo)
        return self.ans
    ```
    
* Car Pooling
    ```python
    def carPooling(self, trips: List[List[int]], capacity: int) -> bool:
        stops = [0 for i in range(1001)]
        for i in trips:
            stops[i[1]]+=i[0]
            stops[i[2]]-=i[0]
        for i in stops:
            capacity-=i
            if capacity<0:
                return False
        return capacity>=0
    ```