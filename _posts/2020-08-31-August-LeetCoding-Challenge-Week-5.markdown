---
layout: post
title:  "August LeetCoding Challenge Week 5"
date:   2020-08-31 20:10:30 +0800
categories: Leetcode
tags : Leetcode Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by August LeetCoding Challenge Week 5.





* Pancake Sorting
    ```python
    def pancakeSort(self, A: List[int]) -> List[int]:
        N = len(A)
        res = []
        for x in range(N, 0, -1):
            i = A.index(x)
            res.extend([i + 1, x])
            A = A[:i:-1] + A[:i]
        return res
    ```
    
* Largest Component Size by Common Factor
    ```python
    class Solution:
        def largestComponentSize(self, A: List[int]) -> int:
            ma = max(A)
            N = len(A)
            m = list(range(ma + 1))
            for a in A:
                for k in range(2, int(math.sqrt(a)) + 1):
                    if a % k == 0:
                        self.u(m, a, k)
                        self.u(m, a, a // k)
            count = collections.defaultdict(int)
            for a in A:
                count[self.f(m, a)] += 1
            return max(count.values())

        def f(self, m, a):
            while m[a] != a:
                m[a] = m[m[a]]
                a = m[a]
            return a

        def u(self, m, a, b):
            if m[a] == m[b]: return
            pa = self.f(m, a)
            pb = self.f(m, b)
            m[pa] = pb
    ```
    
* Delete Node in a BST
    ```python
    def deleteNode(self, root: TreeNode, key: int) -> TreeNode:
        def findMax(root):
            if not root:
                return None
            while root.right:
                root=root.right
            return root
        def findMin(root):
            if not root:
                return None
            while root.left:
                root=root.left
            return root
        if not root:
            return None
        if root.val>key:
            root.left=self.deleteNode(root.left,key)
        elif root.val<key:
            root.right=self.deleteNode(root.right,key)
        else:
            if not root.left:
                return root.right
            elif not root.right:
                return root.left
            else:
                # minNode=findMin(root.right)
                # root.val=minNode.val
                # root.right=self.deleteNode(root.right,root.val)
                maxNode=findMax(root.left)
                root.val=maxNode.val
                root.left=self.deleteNode(root.left,root.val)
        return root
    ```