---
layout: post
title:  "September LeetCoding Challenge Week 2"
date:   2020-09-14 19:19:30 +0800
categories: Leetcode
tags : Leetcode, Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by September LeetCoding Challenge Week 2.




* Sum of Root To Leaf Binary Numbers
    ```python
    class Solution:
        def sumRootToLeaf(self, root: TreeNode) -> int:
            if not root: return 0
            self.res = 0
            self.dfs(root, root.val)
            return self.res

        def dfs(self, root, preSum):
            if not root.left and not root.right:
                self.res = (self.res + preSum) % (10 ** 9 + 7)
                return
            if root.left:
                self.dfs(root.left, preSum * 2 + root.left.val)
            if root.right:
                self.dfs(root.right, preSum * 2 + root.right.val)
    ```
    
* Compare Version Numbers
    ```python
    def compareVersion(self, version1: str, version2: str) -> int:
        v1_split = version1.split('.')
        v2_split = version2.split('.')
        v1_len, v2_len = len(v1_split), len(v2_split)
        maxLen = max(v1_len, v2_len)
        for i in range(maxLen):
            temp1, temp2 = 0, 0
            if i < v1_len:
                temp1 = int(v1_split[i])
            if i < v2_len:
                temp2 = int(v2_split[i])
            if temp1 < temp2:
                return -1
            elif temp1 > temp2:
                return 1
        return 0
    ```
    
* Bulls and Cows
    ```python
    def getHint(self, secret: str, guess: str) -> str:
        bulls = 0
        cows = 0
        secret_dict = collections.defaultdict(int)
        for s, g in zip(secret, guess):
            if s == g:
                bulls += 1
            else:
                secret_dict[s] += 1
        for i, g in enumerate(guess):
            if secret[i] != guess[i] and secret_dict[g]:
                cows += 1
                secret_dict[g] -= 1
        return str(bulls) + "A" + str(cows) + "B"
    ```
    
* Maximum Product Subarray
    ```python
    def maxProduct(self, nums: List[int]) -> int:
        if not nums: return 0
        N = len(nums)
        f = [0] * N
        g = [0] * N
        f[0] = g[0] = res = nums[0]
        for i in range(1, N):
            f[i] = max(f[i - 1] * nums[i], nums[i], g[i - 1] * nums[i])
            g[i] = min(f[i - 1] * nums[i], nums[i], g[i - 1] * nums[i])
            res = max(res, f[i])
        return res
    ```
    
* Combination Sum III
    ```python
    class Solution:
        def combinationSum3(self, k: int, n: int) -> List[List[int]]:
            res = []
            self.dfs(range(1, 10), k, n, 0, res, [])
            return res

        def dfs(self, nums, k, n, index, res, path):
            if k < 0 or n < 0:
                return 
            elif k == 0 and n == 0:
                res.append(path)
                return
            for i in range(index, len(nums)):
                self.dfs(nums, k - 1, n - nums[i], i + 1, res, path + [nums[i]])
    ```
    
* Insert Interval
    ```python
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        intervals.append(newInterval)
        new_intervals = []
        intervals.sort(key=lambda x:x[0])     
         
        for x in intervals:
            if new_intervals and x[0] <= new_intervals[-1][-1]:
                new_intervals[-1][-1] = max(x[-1],new_intervals[-1][-1])
            else:
                new_intervals.append(x)
        return new_intervals
    ```
    
* House Robber
    ```python
    def rob(self, nums: List[int]) -> int:
        now = last = 0
        for i in nums:
            last, now = now, max(i+last, now)
        return now
    ```