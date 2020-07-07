---
layout: post
title:  "July LeetCoding Challenge Week 1"
date:   2020-07-07 23:23:30 +0800
categories: Leetcode
tags : Leetcode, Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by July LeetCoding Challenge Week 1.




* Arranging Coins
    * Math method
        ```python
        def arrangeCoins(self, n: int) -> int:
            return int((-1+math.sqrt(1+8*n))/2)
        ```

* Binary Tree Level Order Traversal II
    ```python
    def levelOrderBottom(self, root: TreeNode) -> List[List[int]]:
        res = []
        queue = [(root, 0)]
        while len(queue) > 0:
            node, depth = queue.pop()
            if node:
                if len(res) <= depth:
                    res.insert(0, [])
                res[-(depth+1)].append(node.val)
                queue.insert(0, (node.left, depth+1))
                queue.insert(0, (node.right, depth+1))
        return res
    ```
    
* Prison Cells After N Days
    ```python
    def prisonAfterNDays(self, oldcells: List[int], N: int) -> List[int]:
        cells = copy.deepcopy(oldcells)
        count = 0
        N %= 14
        if N == 0:
            N = 14
        while count < N:
            newCell = [0] * 8
            for i in range(1, 7):
                if cells[i - 1] == cells[i + 1]:
                    newCell[i] = 1
                else:
                    newCell[i] = 0
            cells = newCell
            count += 1
        return cells

    ```

* Ugly Number II
    * For Loop
        ```python
        def nthUglyNumber(self, n: int) -> int:
            if n < 0:
                return 0
            dp = [1] * n
            index2, index3, index5 = 0, 0, 0
            for i in range(1, n):
                dp[i] = min(2 * dp[index2], 3 * dp[index3], 5 * dp[index5])
                if dp[i] == 2 * dp[index2]: index2 += 1
                if dp[i] == 3 * dp[index3]: index3 += 1
                if dp[i] == 5 * dp[index5]: index5 += 1
            return dp[n - 1]
        ```
    
    * While
        ```python
        def nthUglyNumber(self, n: int) -> int:
            if n < 0:
                return 0
            dp = [1]
            index2, index3, index5 = 0, 0, 0
            while len(dp) < n:
                while dp[-1] >= 2 * dp[index2]: 
                    index2 += 1
                while dp[-1] >= 3 * dp[index3]: 
                    index3 += 1
                while dp[-1] >= 5 * dp[index5]: 
                    index5 += 1
                dp.append(min(dp[index2] * 2 , dp[index3] * 3, dp[index5] * 5))
            return dp[-1]
        ```

* Hamming Distance
    ```python
    def hammingDistance(self, x: int, y: int) -> int:
        return bin(x^y).count('1')
    ```
    
* Plus One
    ```python
    def plusOne(self, digits: List[int]) -> List[int]:
        if digits == []: 
            return digits
        length, carry, i = len(digits), 1, 0
        while carry and i < length:
            num = digits[length - i - 1] + carry
            carry = int(num / 10)
            digits[length - i - 1] = num % 10
            i += 1
        if i == length and carry:
            digits.insert(0, 1)
        return digits
    ```
    
* Island Perimeter
    ```python
    def islandPerimeter(self, grid: List[List[int]]) -> int:
        M, N = len(grid), len(grid[0])
        counts = 0
        neighbors = 0
        for i in range(M):
            for j in range(N):
                if grid[i][j] == 1:
                    counts += 1
                    if i < M - 1:
                        if grid[i + 1][j] == 1:
                            neighbors += 1
                    if j < N - 1:
                        if grid[i][j + 1] == 1:
                            neighbors += 1
        return 4 * counts - 2 * neighbors
    ```