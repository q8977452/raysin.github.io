---
layout: post
title:  "July LeetCoding Challenge Week 4"
date:   2020-07-28 20:44:30 +0800
categories: Leetcode
tags : Leetcode, Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by July LeetCoding Challenge Week 4.





* Binary Tree Zigzag Level Order Traversal
    ```python
    class Solution:
        def preorder(self, root, level, res):
             if root:
                if len(res) < level+1: 
                    res.append([])
                if level % 2 == 0: 
                    res[level].append(root.val)
                else: 
                    res[level].insert(0, root.val) # 向0位置插入
                self.preorder(root.left, level+1, res)
                self.preorder(root.right, level+1, res)
        def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:
            res=[]
            self.preorder(root, 0, res)
            return res
    ```
    
* Single Number III
    ```python
    def singleNumber(self, nums: List[int]) -> List[int]:
        count = collections.Counter(nums)
        res = []
        for num, c in count.items():
            if c == 1:
                res.append(num)
        return res
    ```
    
* All Paths From Source to Target
    ```python
    def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:
        res = []
        def dfs(graph, start, end, res, path):
            if start == end:
                res.append(path)
            for node in graph[start]:
                dfs(graph, node, end, res, path + [node])

        dfs(graph, 0, len(graph) - 1, res, [0])
        return res
    ```
    
* Find Minimum in Rotated Sorted Array II
    * Long coed
        ```python
        def findMin(self, nums: List[int]) -> int:
            lo, hi = 0, len(nums)-1
            while lo < hi: 
                if nums[lo] < nums[hi]: 
                    break 
                mid = (lo + hi)//2
                if nums[lo] == nums[mid] == nums[hi]:
                    lo += 1
                    hi -= 1
                elif nums[lo] <= nums[mid]: lo = mid + 1
                else: hi = mid
            return nums[lo]
        ```
    * Short Code
        ```python
        def findMin(self, nums: List[int]) -> int:
            return min(nums)
        ```

* Add Digits
    * Congruence formula
        ![Method 1](https://wikimedia.org/api/rest_v1/media/math/render/svg/0ef3b4ff4ac20af5790c91886a80773506e09ade)
        ```python
        def addDigits(self, num: int) -> int:
            if num == 0:
                return 0
            elif num % 9 == 0:
                return 9
            else:
                return num % 9
        ```
        ![Method 2](https://wikimedia.org/api/rest_v1/media/math/render/svg/835ce0cbf5d1f5eb0b1859f73812b77e13ddf086)
        ```python
        def addDigits(self, num: int) -> int:
            if num == 0:
                return 0
            else:
                return 1 + (num - 1) % 9
        ```
        
* Construct Binary Tree from Inorder and Postorder
    ```python
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        if not inorder or not postorder: 
            return None
        val = postorder[-1]
        root = TreeNode(val)
        index = inorder.index(val)
        root.left = self.buildTree(inorder[:index], postorder[:index])
        root.right = self.buildTree(inorder[index+1:], postorder[index:-1])
        return root
    ```


* Task Scheduler
    ```python
    def leastInterval(self, tasks: List[str], n: int) -> int:
        count = collections.Counter(tasks)
        most = count.most_common()[0][1]
        num_most = len([i for i, v in count.items() if v == most])
        time = (most - 1) * (n + 1) + num_most
        return max(time, len(tasks))
    ```