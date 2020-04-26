---
layout: post
title:  "30-Day LeetCoding Challenge Week3"
date:   2020-04-27 00:44:30 +0800
categories: Leetcode
tags : Leetcode Python 
mathjax: true
---
* Product of Array Except Self
    * https://blog.csdn.net/fuxuemingzhu/article/details/79325534
    ```python
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        answer = []
        _len = len(nums)
        prod = 1
        for i in range(_len):
            answer.append(prod)
            prod *= nums[i]
        prod = 1
        for i in range(_len - 1, -1, -1):
            answer[i] *= prod
            prod *= nums[i]
        return answer
    ```
* Valid Parenthesis String
    * http://bookshadow.com/weblog/2017/09/17/leetcode-valid-parenthesis-string/
    ```python
    def checkValidString(self, s: str) -> bool:
        old_set = set([0])
        
        for c in s:
            new_set = set()
            
            if c == '(':
                for t in old_set:
                    new_set.add(t + 1)
            
            elif c == ')':
                for t in old_set:
                    if t > 0:
                        new_set.add(t - 1)
                        
            elif c == '*':
                for t in old_set:
                    new_set.add(t + 1)
                    new_set.add(t)
                    if t > 0:
                        new_set.add(t - 1)
            
            old_set = new_set
        
        return 0 in old_set
    ```
* Numbers of Islans
    * https://blog.csdn.net/xiaoxiaoley/java/article/details/82557634
    ```python
    def numIslands(self, grid: List[List[str]]) -> int:
        res = 0
        
        for r in range(len(grid)):
            for c in range(len(grid[0])):
                if grid[r][c] == "1":
                    self.dfs(grid, r, c)
                    res += 1
        return res
    
    def dfs(self, grid, i, j):
        dirs = [[-1, 0], [0, 1], [0, -1], [1, 0]]
        grid[i][j] = "0"
        for dir in dirs:
            nr, nc = i + dir[0], j + dir[1]
            if nr >= 0 and nc >= 0 and nr < len(grid) and nc < len(grid[0]):
                if grid[nr][nc] == "1":
                    self.dfs(grid, nr, nc)
    # def dfs(self,grid,i,j):
    #     grid[i][j] = '0'
    #     if i-1>=0 and grid[i-1][j]=='1':
    #         self.dfs(grid,i-1,j)
    #     if i+1<len(grid) and grid[i+1][j]=='1':
    #         self.dfs(grid,i+1,j)
    #     if j-1>=0 and grid[i][j-1]=='1':
    #         self.dfs(grid,i,j-1)
    #     if j+1<len(grid[0]) and grid[i][j+1]=='1':
    #         self.dfs(grid,i,j+1)
    ```

* Minimum Path Sum
    * https://blog.csdn.net/fuxuemingzhu/article/details/82620422
    ```python
    def minPathSum(self, grid: List[List[int]]) -> int:
        
        if not grid or not grid[0]: 
            return 0
        
        m, n = len(grid), len(grid[0])
        
        for i in range(m):
            for j in range(n):
                if i == 0 and j == 0:
                    before = 0
                elif i == 0:
                    before = grid[i][j-1]
                elif j == 0:
                    before = grid[i-1][j]
                else:
                    before = min(grid[i-1][j], grid[i][j-1])
                grid[i][j] += before
        
        return grid[m-1][n-1]
    ```
* Search in Rotated Sorted Array
    * https://www.tutorialspoint.com/search-in-rotated-sorted-array-in-python
    ```python
    def search(self, nums: List[int], target: int) -> int:
        low = 0
        high = len(nums)
        while low<high:
            mid = low + (high-low)//2
            if nums[mid] == target:
                return mid
            if nums[low]<=nums[mid]:
                if target >=nums[low] and target <nums[mid]:
                    high = mid
                else:
                    low = mid+1
            else:
                if target<=nums[high-1] and target>nums[mid]:
                    low = mid+1
                else:
                    high = mid
        return -1
    ```
* Construct Binary Search Tree from Preorder Traversal
    * https://www.tutorialspoint.com/construct-binary-search-tree-from-preorder-traversal-in-python
    ```python
    def bstFromPreorder(self, preorder: List[int]) -> TreeNode:
        root = TreeNode(preorder[0])
        stack = [root]
        for i in preorder[1:]:
            i = TreeNode(i)
            if i.val<stack[-1].val:
                stack[-1].left = i
                stack.append(i)
            else:
                while stack and stack[-1].val<i.val:
                    last = stack.pop(-1)
                last.right = i
                stack.append(i)
        return root
    ```
* Leftmost Column with at Least a One
    * https://blog.csdn.net/erzhushashade/article/details/105660678
    ```python
    
    def leftMostColumnWithOne(self, binaryMatrix: 'BinaryMatrix') -> int:
        dimensions = binaryMatrix.dimensions()
        n = dimensions[0]
        m = dimensions[1] - 1
        height = 0
        leftmost = -1
        while height < n and m >=0:
            if binaryMatrix.get(height, m) == 0:
                height += 1
            else:
                leftmost = m
                m -= 1
        return leftmost
    ```