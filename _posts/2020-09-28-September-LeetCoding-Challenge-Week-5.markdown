---
layout: post
title:  "September LeetCoding Challenge Week 5"
date:   2020-09-31 20:30:30 +0800
categories: Leetcode
tags : Leetcode, Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by September LeetCoding Challenge Week 5.




* Word Break
    ```python
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        dp = [False] * (len(s) + 1)
        dp[0] = True
        for i in range(1, len(s) + 1):
            for k in range(i):
                if dp[k] and s[k:i] in wordDict:
                    dp[i] = True
                    print(dp)
        return dp.pop()
    ```
    
    
* First Missing Positive
    ```python
    def firstMissingPositive(self, nums: List[int]) -> int:
        n = len(nums)
        for i in range(n):
            while nums[i] > 0 and nums[i] <= n and nums[i] != i + 1 and nums[i] != nums[nums[i] - 1]:
                nums[nums[i] - 1], nums[i] = nums[i], nums[nums[i] - 1]
        for i in range(n):
            if i + 1 != nums[i]:
                return i + 1
        return n + 1
    ```