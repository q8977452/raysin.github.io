---
layout: post
title:  "September LeetCoding Challenge Week 4"
date:   2020-09-28 22:35:30 +0800
categories: Leetcode
tags : Leetcode, Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by September LeetCoding Challenge Week 4.




* Majority Element II
    ```python
    def majorityElement(self, nums: List[int]) -> List[int]:
        N = len(nums)
        m = n = cm = cn = 0
        for num in nums:
            if num == m:
                cm += 1
            elif num == n:
                cn += 1
            elif cm == 0:
                m = num
                cm = 1
            elif cn == 0:
                n = num
                cn = 1
            else:
                cm -= 1
                cn -= 1
        cm = cn = 0
        for num in nums:
            if num == m:
                cm += 1
            elif num == n:
                cn += 1
        res = []
        if cm > N / 3:
            res.append(m)
        if cn > N / 3:
            res.append(n)
        return res
    ```
    
* Gas Station
    ```python
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        if sum(gas) < sum(cost): 
            return -1
        n = len(gas)
        diff = 0
        stationIndex = 0
        for i in range(n):
            if gas[i]+diff < cost[i]: 
                stationIndex = i+1
                diff = 0
            else: 
                diff += gas[i]-cost[i]
        return stationIndex
    ```
    
* Find the Difference
    * xor
        ```python
        def findTheDifference(self, s: str, t: str) -> str:
            return chr(reduce(operator.xor, map(ord, s + t)))
        ```
    * +-法
        ```python
        def findTheDifference(self, s: str, t: str) -> str:
            res = ord(t[-1])
            for i in range(len(s)):
                res = res + ord(t[i]) - ord(s[i])
            return chr(res)
        ```
        
* Largest Number
    ```python
    class compare(str):
        def __lt__(x, y):
            return x+y > y+x


    class Solution:
        def largestNumber(self, nums: List[int]) -> str:
            largest = sorted([str(v) for v in nums], key=compare)
            largest = ''.join(largest)

            return '0' if largest[0] == '0' else largest
    ```
    
* Teemo Attacking
    * 如果兩次中毒時間不存在交叉，那麼總的中毒時間易求出。再用總的中毒時間-兩次中毒之間重疊的時間，就是答案。
        ```python
        def findPoisonedDuration(self, timeSeries: List[int], duration: int) -> int:
            if not timeSeries:
                return 0
            total = duration * len(timeSeries)
            for i in range(1, len(timeSeries)):
                if timeSeries[i] < timeSeries[i - 1] + duration:
                    total -= timeSeries[i - 1] + duration - timeSeries[i]
            return total
        ```
    * 使用start和end來表示這次中毒的開始和結束時間。需要判斷新一次中毒的時候，是否還在上一次的中毒週期內
        ```python
        def findPoisonedDuration(self, timeSeries: List[int], duration: int) -> int:
            if len(timeSeries) == 0 :
                return 0
            start = timeSeries[0]
            end, res = start + duration, 0
            for i in range(1, len(timeSeries)):
                time = timeSeries[i]
                if end > time:
                    res += time - start
                else:
                    res += duration
                end = time + duration
                start = time
            res += duration
            return res
        ```
        
* Evaluate Division
    ```python
    def calcEquation(self, equations: List[List[str]], values: List[float], queries: List[List[str]]) -> List[float]:
        table = collections.defaultdict(dict)
        for (x, y), value in zip(equations, values):
            table[x][y] = value
            table[y][x] = 1.0 / value
        
        def dfs(x, y, table, visited):
            if x == y:
                return 1.0
            visited.add(x)
            for n in table[x]:
                if n in visited: continue
                visited.add(n)
                d = dfs(n, y, table, visited)
                if d > 0:
                    return d * table[x][n]
            return -1.0
        
        ans = [dfs(x, y, table, set()) if x in table and y in table else -1.0 for (x, y) in queries]
        return ans
    ```
    
* Subarray Product Less Than K
    ```python
    def numSubarrayProductLessThanK(self, nums: List[int], k: int) -> int:
        N = len(nums)
        prod = 1
        l, r = 0, 0
        res = 0
        while r < N:
            prod *= nums[r]
            while l <= r and prod >= k:
                prod /= nums[l]
                l += 1
            res += r - l + 1
            r += 1
        return res
    ```