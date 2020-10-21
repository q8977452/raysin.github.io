---
layout: post
title:  "October LeetCoding Challenge Week 3"
date:   2020-10-21 17:52:30 +0800
categories: Leetcode
tags : Leetcode, Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by October LeetCoding Challenge Week 3.





* Rotate Array
    每次把數組最後k位交換到正確的位置，循環直到所有元素位置正確。
    ```python
    def rotate(self, nums: List[int], k: int) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        k, start, n = k % len(nums), 0, len(nums)
        while k % n != 0 and n > 0:
            for i in range(k):
                nums[start + i], nums[len(nums) - k + i] = nums[len(nums) - k + i], nums[start + i]
            start, n = start + k, n - k
            k = k % n
    ```
    
* Search a 2D Matrix
    * 左下或者右上開始查找
        ```python
        def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
            if not matrix or not matrix[0]:
                return False
            rows = len(matrix)
            cols = len(matrix[0])
            row, col = 0, cols - 1
            while True:
                if row < rows and col >= 0:
                    if matrix[row][col] == target:
                        return True
                    elif matrix[row][col] < target:
                        row += 1
                    else:
                        col -= 1
                else:
                    return False
        ```
    * Numpy
        ```python
        import numpy as np
        class Solution:
            def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
                matrix = np.reshape(matrix, [1, -1])
                return target in matrix
        ```
        
* Repeated DNA Sequences
    ```python
    def findRepeatedDnaSequences(self, s: str) -> List[str]:
        seen = set()
        repeated = set()
        N = len(s)
        for i in range(N):
            cur = s[i : i+ 10]
            if cur in seen:
                repeated.add(cur)
            else:
                seen.add(cur)
        return list(repeated)
    ```
    
* Best Time to Buy and Sell Stock IV
    ```python
    def maxProfit(self, k: int, prices: List[int]) -> int:
        if k <= 0 or not prices: return 0
        N = len(prices)
        if k >= N:
            _sum = 0
            for i in range(1, N):
                if prices[i] > prices[i - 1]:
                    _sum += prices[i] - prices[i - 1]
            return _sum
        g = [0] * (k + 1)
        l = [0] * (k + 1)
        for i in range(N - 1):
            diff = prices[i + 1] - prices[i]
            for j in range(k, 0, -1):
                l[j] = max(g[j - 1] + max(diff, 0), l[j] + diff)
                g[j] = max(l[j], g[j])
        return g[-1]
    ```
    
*  Minimum Domino Rotations For Equal Row
    * 分析
        * 先把A,B兩面的數字做一個統計，如果出現最多的點數<牌的個數，那麼無論如何都無法翻轉成功。
        * 如果出現最多的點數=牌的個數，那麼這個時候需要保證每一個牌的都有且只有一面的點數是這個最多的點數。
        * 如果出現最多的點數>牌的個數，那麼需要保證每個牌都至少有一面是這個數字，此時兩面都是這個數字的話，就不用翻轉
    * 程式
        ```python
        def minDominoRotations(self, A: List[int], B: List[int]) -> int:
            N = len(A)
            count = collections.Counter(A + B)
            if count.most_common(1)[0][1] < N:
                return -1
            target = count.most_common(1)[0][0]
            a_swap = 0
            b_swap = 0
            for i in range(N):
                if A[i] == B[i]:
                    if A[i] == target:
                        continue
                    else:
                        return -1
                elif A[i] == target:
                    b_swap += 1
                elif B[i] == target:
                    a_swap += 1
                else:
                    return -1
            return min(a_swap, b_swap)
        ```
        
* Clone Graph
    * DFS
        * 直接把遍歷過的對象和復制出來的對像一一對應即可
        * 遍歷到一個新的節點的時候，需要判斷這個節點是否在字典中出現過，
            * 如果出現過就把它對應的複制出來的對象放到其neighboors裡
            * 若沒有出現過，那麼就重新構造該節點，並把原節點和該節點放到字典中保存
    ```python
    class Solution:
        def cloneGraph(self, node: 'Node') -> 'Node':
            node_copy = self.dfs(node, dict())
            return node_copy

        def dfs(self, node, hashd):
            if not node: return None
            if node in hashd: return hashd[node]
            node_copy = Node(node.val, [])
            hashd[node] = node_copy
            for n in node.neighbors:
                n_copy = self.dfs(n, hashd)
                if n_copy:
                    node_copy.neighbors.append(n_copy)
            return node_copy
    ```
    * BFS
        * 使用了字典保存每一個對應關係
        * 當新構造出一個節點之後，必須同時把它放到字典中保存
        * 每遍歷到一個節點時，都要把它的所有鄰居放到隊列中
    ```python
    def cloneGraph(self, node: 'Node') -> 'Node':
        if not node: return None
        que = collections.deque()
        hashd = dict()
        que.append(node)
        node_copy = Node(node.val, [])
        hashd[node] = node_copy
        while que:
            t = que.popleft()
            if not t: continue
            for n in t.neighbors:
                if n not in hashd:
                    hashd[n] = Node(n.val, [])
                    que.append(n)
                hashd[t].neighbors.append(hashd[n])
        return node_copy   
    ```
    
* Asteroid Collision
    * 分析
        * 從左到右遍歷每個行星，並和棧頂數字相比較，當棧頂數字為正（向右），當前數字為負（向左）的時候，會發生碰撞
        * 這時候，判斷遺留下來的數字是多少，保存到ast裡
            * 如果ast為空代表啥都沒有了
            * 如果ast質量大於棧頂元素會留下來ast，否則留下pre
            * 判斷ast是否為空，不為空就把遺留下來的數字進棧就好了
    * 程式
    ```python
    def asteroidCollision(self, asteroids: List[int]) -> List[int]:
        stack = []
        for ast in asteroids:
            while stack and ast < 0 and stack[-1] >= 0:
                pre = stack.pop()
                if ast == -pre:
                    ast = None
                    break
                elif -ast < pre:
                    ast = pre
            if ast != None:
                stack.append(ast)
        return stack
    ```