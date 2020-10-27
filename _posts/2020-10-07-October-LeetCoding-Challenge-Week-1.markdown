---
layout: post
title:  "October LeetCoding Challenge Week 1"
date:   2020-10-07 18:04:30 +0800
categories: Leetcode
tags : Leetcode Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by October LeetCoding Challenge Week 1.





 * Number of Recent Calls
     ```python
     class RecentCounter:

         def __init__(self):
             self.que = collections.deque()

         def ping(self, t):
             """
             :type t: int
             :rtype: int
             """
             while self.que and self.que[0] < t - 3000:
                 self.que.popleft()
             self.que.append(t)
             return len(self.que)
     ```
     
* Combination Sum
    ```python
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []
        candidates.sort()
        def dfs(nums, target, index, res, path):
            if target < 0:
                return
            elif target == 0:
                res.append(path)
                return
            for i in range(index, len(nums)):
                if nums[index] > target:
                    return
                dfs(nums, target - nums[i], i, res, path + [nums[i]])
        dfs(candidates, target, 0, res, [])
        return res
    ```
    
* K-diff Pairs in an Array
    ```python
    def findPairs(self, nums: List[int], k: int) -> int:
        res = 0
        if k < 0: return 0
        elif k == 0:
            count = collections.Counter(nums)
            for n, v in count.items():
                if v >= 2:
                    res += 1
            return res
        else:
            nums = set(nums)
            for num in nums:
                if num + k in nums:
                    res += 1
            return res
    ```
    
* Remove Covered Intervals
    ```python
    def removeCoveredIntervals(self, intervals: List[List[int]]) -> int:
        dic = {}
        for i in range(len(intervals)):
            for j in range(i+1,len(intervals)):
                if i == j :continue
                if intervals[i][0] <= intervals[j][0] and intervals[i][1] >= intervals[j][1]:
                    dic[j] = 1
                elif intervals[i][0] >= intervals[j][0] and intervals[i][1] <= intervals[j][1]:
                    dic[i] = 1
        return len(intervals) - len(dic)
    ```
    
* Complement of Base 10 Integer
    ```python
    def bitwiseComplement(self, N: int) -> int:
        if N == 0: 
            return 1
        x = 1 
        while x <= N: 
            x <<= 1 
        return (x - 1) ^ N
    ```
    
* Insert into a Binary Search Tree
    ```python
    def insertIntoBST(self, root: TreeNode, val: int) -> TreeNode:
        if not root:
            return TreeNode(val)
        if val > root.val:
            root.right = self.insertIntoBST(root.right, val)
        if val < root.val:
            root.left = self.insertIntoBST(root.left, val)
        return root
    ```
    
* Rotate List
    ```python
    def rotateRight(self, head: ListNode, k: int) -> ListNode:
        if not head or not head.next: 
                return head
        _len = 0
        root = head
        while head:
            _len += 1
            head = head.next
        k %= _len
        if k == 0: 
            return root
        fast, slow = root, root
        while k - 1:
            fast = fast.next
            k -= 1
        pre = slow
        while fast.next:
            fast = fast.next
            pre = slow
            slow = slow.next
        pre.next = None
        fast.next = root
        return slow
    ```