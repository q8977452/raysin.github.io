---
layout: post
title:  "June LeetCoding Challenge Week 1"
date:   2020-06-07 18:15:30 +0800
categories: Leetcode
tags : Leetcode, Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by June LeetCoding Challenge Week 1.




* Invert Binary Tree
    * Recurrent
    ```python
    def invertTree(self, root: TreeNode) -> TreeNode:
        if root == None:
            return root
        tmp = root.left
        root.left = self.invertTree(root.right)
        root.right = self.invertTree(tmp)
        return root
    ```
    * BFS
    ```python
    def invertTree(self, root: TreeNode) -> TreeNode:
        if root == None:
            return root
        q = [root]
        while len(q) != 0:
            q[0].left, q[0].right = q[0].right, q[0].left
            if q[0].left:
                q.append(q[0].left)
            if q[0].right:
                q.append(q[0].right)
            del q[0]

        return root
    ```
    
* Delete Node in a Linked List
    ```python
     def deleteNode(self, node):
        """
        :type node: ListNode
        :rtype: void Do not return anything, modify node in-place instead.
        """
        node.val = node.next.val
        node.next = node.next.next
    ```
    
* Two City Scheduling
    ```python
    def twoCitySchedCost(self, costs: List[List[int]]) -> int:
        N = len(costs)
        diff = [c[0] - c[1] for c in costs]
        indices = sorted(range(N), key = lambda k:diff[k])
        result = 0
        for i in range(int(N/2)):
            result += costs[indices[i]][0]
            result += costs[indices[N-i-1]][1]
        return result
    ```
    
* Reverse String
    ```python
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        for index in range(len(s) // 2):
            s[index], s[-index - 1] = s[-index - 1], s[index]
    ```
    
* Random Pick with Weight
    ```python
    class Solution:

        def __init__(self, w: List[int]):
            self.preSum = [0] * len(w)
            self.preSum[0] = w[0]
            for i in range(1, len(w)):
                self.preSum[i] = self.preSum[i - 1] + w[i] 

        def pickIndex(self) -> int:
            total = self.preSum[-1]
            rand = random.randint(0, total -1)
            left, right = 0, len(self.preSum) - 1
            while left + 1 < right:
                mid = (left + right) // 2
                if rand >= self.preSum[mid]:
                    left = mid
                else:
                    right = mid
            if rand < self.preSum[left]:
                return left
            return right
    ```
    
* Queue Reconstruction by Height
    ```python
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        people.sort(key = lambda x : (-x[0], x[1]))
        res = []
        for p in people:
            res.insert(p[1], p)
        return res
    ```
    
* Coin Change 2
    ```python
    def change(self, amount: int, coins: List[int]) -> int:
        dp = [0] * (amount + 1)
        dp[0] = 1
        for coin in coins:
            for i in range(1, amount + 1):
                if coin <= i:
                    dp[i] += dp[i - coin]
        return dp[amount]
    ```