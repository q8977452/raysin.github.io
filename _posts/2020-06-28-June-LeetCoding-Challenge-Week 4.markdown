---
layout: post
title:  "June LeetCoding Challenge Week 4"
date:   2020-06-28 19:54:30 +0800
categories: Leetcode
tags : Leetcode Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by June LeetCoding Challenge Week 4.




* Single Number II
    ```python
    def singleNumber(self, nums: List[int]) -> int:
        res = 0
        for i in range(32):
            cnt = 0
            mask = 1 << i
            for num in nums:
                if num & mask:
                    cnt += 1
            if cnt % 3 == 1:
                res |= mask
        if res >= 2 ** 31:
            res -= 2 ** 32
        return res
    ```
* Count Complete Tree Nodes
    ```python
    def countNodes(self, root: TreeNode) -> int:
        if not root:
            return 0
        nodes = 0
        left_height = self.getHeight(root.left)
        right_height = self.getHeight(root.right)
        if left_height == right_height:
            nodes = 2 ** left_height + self.countNodes(root.right)
        else:
            nodes = 2 ** right_height + self.countNodes(root.left)
        return nodes
    
    def getHeight(self, root):
        height = 0
        while root:
            height += 1
            root = root.left
        return height
    ```
    
* Unique Binary Search Trees
    ```python
    class Solution:
        def __init__(self):
            self.dp = dict()
        
        def numTrees(self, n: int) -> int:
            if n in self.dp:
                return self.dp[n]
            if n == 0 or n == 1:
                return 1
            ans = 0
            for i in range(1, n + 1):
                ans += self.numTrees(i - 1) * self.numTrees(n - i)
            self.dp[n] = ans
            return ans
    ```
* Find the Duplicate Number
    ```python
    def findDuplicate(self, nums: List[int]) -> int:
        slow = nums[0]
        fast = nums[nums[0]]
        while slow != fast:
            fast = nums[nums[fast]]
            slow = nums[slow]
        fast = 0
        while slow != fast:
            fast = nums[fast]
            slow = nums[slow]
        return fast
    ```
* Sum Root to Leaf Numbers
    ```python
    class Solution:
        def sumNumbers(self, root: TreeNode) -> int:
            if root == None:
                return 0
            res = [0]
            self.dfs(root, res, root.val)
            return res[0]

        def dfs(self, root, res, path):
            if root.left == None and root.right == None:
                res[0] += path
            if root.left != None:
                self.dfs(root.left, res, path * 10 + root.left.val)
            if root.right != None:
                self.dfs(root.right, res, path * 10 + root.right.val)
    ```

* Perfect Squares
    ```python
    def numSquares(self, n: int) -> int:
        if n == 0:
            return 0
        output = [0x7fffffff]*(n+1)
        output[0] = 0
        output[1] = 1
        for i in range(2, n+1):
            j = 1
            while(j*j<=i):
                output[i] = min(output[i], output[i-j*j]+1)
                j += 1
        
        return output[n]
    ```
* Reconstruct Itinerary
    ```python
    class Solution:
        def findItinerary(self, tickets: List[List[str]]) -> List[str]:
            graph = collections.defaultdict(list)
            for frm, to in tickets:
                graph[frm].append(to)
            for frm, tos in graph.items():
                tos.sort(reverse=True)
            res = []
            self.dfs(graph, "JFK", res)
            return res[::-1]

        def dfs(self, graph, source, res):
            while graph[source]:
                v = graph[source].pop()
                self.dfs(graph, v, res)
            res.append(source)
    ```