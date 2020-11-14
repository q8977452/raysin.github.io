---
layout: post
title:  "November LeetCoding Challenge Week 2"
date:   2020-11-14 17:22:30 +0800
categories: Leetcode
tags : Leetcode Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by November LeetCoding Challenge Week 2.


* Binary Tree Tilt
    * 後續遍歷
        ```python
        class Solution:

            def findTilt(self, root: TreeNode) -> int:
                self.rst = 0
                self.GetTotal(root)
                return self.rst

            def GetTotal(self, node : TreeNode) -> int:

                if node == None: return 0
                left = self.GetTotal(node.left)
                right = self.GetTotal(node.right)
                self.rst += abs(left - right)
                return node.val + left + right
        ```
        
* Maximum Difference Between Node and Ancestor
    * 想法
        * cur_max記錄當前祖先的最大值
        * cur_min記錄當前祖先的最小值
        * 遞歸的每一個結點都跟最大值和最小值做絕對值差
        * 然後取其中的最大絕對值差就行了
    * 程式
        ```python
        class Solution:
            def solve(self,root,cur_max,cur_min):
                if root is None:
                    return 
                self.res=max(self.res,abs(cur_max-root.val),abs(cur_min-root.val))
                cur_min=min(cur_min,root.val)
                cur_max=max(cur_max,root.val)

                self.solve(root.left,cur_max,cur_min)
                self.solve(root.right,cur_max,cur_min)
            def maxAncestorDiff(self, root: TreeNode) -> int:
                self.res=0
                if root is None:
                    return self.res
                self.solve(root,root.val,root.val)
                return self.res
        ```
        
* Flipping an Image
    * 翻轉 + 異或
        ```python
        def flipAndInvertImage(self, A: List[List[int]]) -> List[List[int]]:
            rows = len(A)
            cols = len(A[0])
            for row in range(rows):
                A[row] = A[row][::-1]
                for col in range(cols):
                    A[row][col] ^= 1
            return A
        ```
* Valid Square
    ```python
    def validSquare(self, p1: List[int], p2: List[int], p3: List[int], p4: List[int]) -> bool:
        def d(point1, point2):
            return (point1[0] - point2[0]) ** 2 + (point1[1] - point2[1]) ** 2
        s = set([d(p1, p2), d(p1, p3), d(p1, p4), d(p2, p3), d(p2, p4), d(p3, p4)])
        return 0 not in s and len(s) == 2
    ```
    
* Permutations II
    ```python
    class Solution:
        def permuteUnique(self, nums: List[int]) -> List[List[int]]:
            res = []
            self.helper(nums, res, [])
            return res

        def helper(self, nums, res, path):
            if not nums and path not in res:
                res.append(path)
            else:
                for i in range(len(nums)):
                    self.helper(nums[:i] + nums[i+1:], res, path + [nums[i]])
    ```

* Populating Next Right Pointers in Each Node
    * 遞迴
        ```python
        def connect(self, root: 'Node') -> 'Node':
            if root and root.left:
                root.left.next = root.right
                root.right.next = root.next.left if root.next else None
                self.connect(root.left)
                self.connect(root.right)
            return root
        ```
        
* Poor Pigs
    ```python
    def poorPigs(self, buckets: int, minutesToDie: int, minutesToTest: int) -> int:
        tests = minutesToTest / minutesToDie + 1
        pigs = 0
        while tests ** pigs < buckets:
            pigs += 1
        return pigs
    ```