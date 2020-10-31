---
layout: post
title:  "October LeetCoding Challenge Week 5"
date:   2020-10-31 18:38:30 +0800
categories: Leetcode
tags : Leetcode Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by October LeetCoding Challenge Week 5.




* Maximize Distance to Closest Person
    * 分析
        * 步驟
            * 第一步
                * 假設在很遠的位置有個座位有人坐，那麼從左到右開始遍歷，找出每個座位到其最近的左邊的有人坐的位置的距離
            * 第二步
                * 假設在很遠的位置有個座位有人坐，那麼從右到左開始遍歷，找出每個字符到其最近的右邊的有人坐的位置的距離，並和第一步求出的距離進行比較，找出最小值為結果
            * 最後
                * 找出這個列表的最大值

        * 技巧
            * 設了一個比字符串長度更遠的一個字符C，保證後面求最小值更新距離時一定會被更新
            * 無論如何都用到了abs求絕對值，哪怕可能是不需要的，目的是不用費腦子思考誰大誰小了
    * 程式
        ```python
        def maxDistToClosest(self, seats: List[int]) -> int:
            index = -20000
            _len = len(seats)
            ans = [0] * _len
            for i in range(_len):
                if seats[i] == 1:
                    index = i
                else:
                    ans[i] = abs(i - index)
            index = -20000
            for i in range(_len - 1, -1, -1):
                if seats[i] == 1:
                    index = i
                else:
                    ans[i] = min(abs(i - index), ans[i])
            return max(ans)
        ```
        
* Number of Longest Increasing Subsequence
    * 分析
        * 在每個位置記錄當前的LIS和能得到該LIS長度的子序列數目
        * 在對每個位置進行計算的時候，我們都要找到該位置的LIS長度，並對能得到該長度的子序列的個數進行求和
        * 最後，我們需要對所有位置等於LIS長度的進行統計
    * 程式
        ```python
        def findNumberOfLIS(self, nums: List[int]) -> int:
            dp, longest = [[1, 1] for _ in range(len(nums))], 1
            for i in range(1, len(nums)):
                curr_longest, count = 1, 0
                for j in range(i):
                    if nums[j] < nums[i]:
                        curr_longest = max(curr_longest, dp[j][0] + 1)
                for j in range(i):
                    if dp[j][0] == curr_longest - 1 and nums[j] < nums[i]:
                        count += dp[j][1]
                dp[i] = [curr_longest, max(count, dp[i][1])]
                longest = max(longest, curr_longest)
            return sum([item[1] for item in dp if item[0] == longest])
        ```
        
* Recover Binary Search Tree
    ```python
    class Solution:
        def recoverTree(self, root: TreeNode) -> None:
            """
            Do not return anything, modify root in-place instead.
            """
            self.pre, self.first, self.second = None, None, None
            self.inOrder(root)
            self.first.val, self.second.val = self.second.val, self.first.val

        def inOrder(self, root):
            if not root: return
            self.inOrder(root.left)
            if self.pre and self.pre.val > root.val:
                if not self.first:
                    self.first = self.pre
                self.second = root
            self.pre = root
            self.inOrder(root.right)
    ```