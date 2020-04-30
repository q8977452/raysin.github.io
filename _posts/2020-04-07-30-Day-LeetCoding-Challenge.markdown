---
layout: post
title:  "30-Day LeetCoding Challenge Week1"
date:   2020-04-07 23:18:30 +0800
categories: Leetcode
tags : Leetcode Python
mathjax: true
---
* content 
{:toc}
Practice Coding by 30-Day LeetCoding Challenge Week1.




* Single Number
    ```python
    def singleNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        a = []
        for x in nums:
            if x not in a:
                a.append(x)
        return 2*sum(a) - sum(nums)
    ```
* Happy Number
    ```python
    def isHappy(self, n):
        """
        :type n: int
        :rtype: bool
        """
        a = []
        while n not in a:
            a.append(n)
            n = sum([int(x)**2 for x in str(n)])
        return True if n == 1 else False
    ```
* Maximum Subarray
    ```python
    def maxSubArray(self, nums: List[int]) -> int:
        l = g = -2147483647-1
        for n in nums:
            l = max(n,l+n)
            g = max(l,g)
        return g
    ```
* Move Zeroes
    ```python
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        nums.sort(key = lambda x: 1 if x == 0 else 0)

    ```
* Best Time to Buy and Sell Stock II
    ```python
    def maxProfit(self, prices: List[int]) -> int:
        profit = 0
        length = len(prices)
        for i in range(0,length-1):
            if prices[i+1] > prices[i]:
                profit += prices[i+1] - prices[i]
        return profit
    ```
* Group Anagrams
    ```python
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        temp_dict = {}
        for word in strs:
            word_key = "".join(sorted(word))
            if word_key not in temp_dict:
                temp_dict[word_key] = [word]
            else:
                temp_dict[word_key].append(word)
            
        result = []
        for value in temp_dict.values():
            result += [value]
        return result
    ```
* Counting Elements
    ```python
    def countElements(self, arr: List[int]) -> int:
        tmp = {}
        for i in arr:
            tmp[i] = 1

        result = 0
        for x in arr:
            if x+1 in tmp:
                result += 1
        return result
    ```