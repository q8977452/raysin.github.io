---
layout: post
title:  "July LeetCoding Challenge Week 5"
date:   2020-07-31 21:42:30 +0800
categories: Leetcode
tags : Leetcode Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by July LeetCoding Challenge Week 5.





* Best Time to Buy and Sell Stock with Cooldown
    ```python
    def maxProfit(self, prices: List[int]) -> int:
        if not prices: 
            return 0
        prev_cash = 0
        curr_cash = 0
        hold = -prices[0]
        for i in range(1, len(prices)):
            temp = curr_cash
            curr_cash = max(curr_cash, hold + prices[i])
            hold = max(hold, (prev_cash if i >= 2 else 0) - prices[i])
            prev_cash = temp
        return curr_cash
    ```
    
    
* Word Break II

    ```python
    class Solution:
        def wordBreak(self, s: str, wordDict: List[str]) -> List[str]:
            res = []
            memo = dict()
            return self.dfs(s, res, wordDict, memo)

        def dfs(self, s, res, wordDict, memo):
            if s in memo: return memo[s]
            if not s:
                return [""]
            res = []
            for word in wordDict:
                if s[:len(word)] != word: continue
                for r in self.dfs(s[len(word):], res, wordDict, memo):
                    res.append(word + ("" if not r else " " + r))
            memo[s] = res
            return res
    ```
    
* Climbing Stairs
    ```python
    def climbStairs(self, n: int) -> int:
        pre = cur = 1
        for i in range(1, n):
            pre, cur = cur, pre+cur
        return cur
    ```