---
layout: post
title:  "June LeetCoding Challenge Week 2"
date:   2020-06-14 18:40:30 +0800
categories: Leetcode
tags : Leetcode, Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by June LeetCoding Challenge Week 2.




* Power of Two
    ```python
    def isPowerOfTwo(self, n: int) -> bool:
        tmp = 1
        while n >= tmp:
            if n == tmp:
                return True
            tmp *=2
        return False
    ```
*  Is Subsequence
    * Queue
    ```python
    def isSubsequence(self, s: str, t: str) -> bool:
        queue = collections.deque(s)
        for c in t:
            if not queue:
                return True
            if c == queue[0]:
                queue.popleft()
        return not queue
    ```
    * Two indicator
    ```python
    def isSubsequence(self, s: str, t: str) -> bool:
        si, ti = 0, 0
        while si < len(s) and ti < len(t):
            if t[ti] == s[si]:
                si += 1
            ti += 1
        return si == len(s)
    ```
    
* Search Insert Position
    * 64 ms
    ```python
    def searchInsert(self, nums: List[int], target: int) -> int:
        pos = 0
        for i in nums:
            if i == target:
                return pos
            elif i < target:
                pos += 1
            else:
                return pos
        return pos
    ```
    * 48 ms
    ```python
    def searchInsert(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums)-1
        while l <= r :
            # m = (l+r) // 2
            m = (l+r) >> 1
            if target == nums[m] : 
                return m
            if target < nums[m] :
                r = m-1
            else :
                l = m+1
            
        return l
    ```

* Insert Delete GetRandom O(1)
    ```python
    class RandomizedSet:

        def __init__(self):
            """
            Initialize your data structure here.
            """
            self.set = set()
            self.size = 0

        def insert(self, val: int) -> bool:
            """
            Inserts a value to the set. Returns true if the set did not already contain the specified element.
            """
            if val not in self.set:
                self.set.add(val)
                self.size += 1
                return True
            return False

        def remove(self, val: int) -> bool:
            """
            Removes a value from the set. Returns true if the set contained the specified element.
            """
            if val in self.set:
                self.set.remove(val)
                self.size -= 1
                return True
            return False

        def getRandom(self) -> int:
            """
            Get a random element from the set.
            """
            ind = random.randint(0, self.size - 1)
            return list(self.set)[ind]
    ```
    
* Largest Divisible Subset
    ```python
    def largestDivisibleSubset(self, nums: List[int]) -> List[int]:
        if not nums:
            return []
        N = len(nums)
        nums.sort()
        dp = [0] * N
        parent = [0] * N
        mx = 0
        mx_index = -1
        for i in range(N):
            for j in range(i - 1, -1, -1):
                if nums[i] % nums[j] == 0 and dp[i] < dp[j] + 1:
                    dp[i] = dp[j] + 1
                    parent[i] = j
                    if dp[i] > mx:
                        mx = dp[i]
                        mx_index = i
        res = list()
        for k in range(mx + 1):
            res.append(nums[mx_index])
            mx_index = parent[mx_index]
        return res[::-1]
    ```
    
* Cheapest Flights Within K Stops
    ```python
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, K: int) -> int:
        graph = collections.defaultdict(dict)
        for u, v, e in flights:
            graph[u][v] = e
        ans = float('inf')
        que = collections.deque()
        que.append((src, 0))
        step = 0
        while que:
            size = len(que)
            for i in range(size):
                cur, cost = que.popleft()
                if cur == dst:
                    ans = min(ans, cost)
                for v, w in graph[cur].items():
                    if cost + w > ans:
                        continue
                    que.append((v, cost + w))
            if step > K:
                break
            step += 1
        return -1 if ans == float('inf') else ans
    ```