---
layout: post
title:  "May LeetCoding Challenge Week 4"
date:   2020-05-28 22:00:30 +0800
categories: Leetcode
tags : Leetcode Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by May LeetCoding Challenge Week 4.




* Sort Characters By Frequency
    ```python
    def frequencySort(self, s: str) -> str:
        return ''.join(c*x for x,c in Counter(s).most_common())
    ```
    
* Interval List Intersections
    ```python
    def intervalIntersection(self, A: List[List[int]], B: List[List[int]]) -> List[List[int]]:
        res = []
        m, n =len(A), len(B)
        if m ==0 or n == 0:
            return res
        ai, bi = 0, 0
        while ai != m and bi != n :
            a = A[ai]
            b = B[bi]
            if a[1] < b[0]:
                ai += 1
            elif a[0] > b[1]:
                bi += 1
            else:
                res.append([max(a[0],b[0]), min(a[1], b[1])])
                if a[1] <= b[1]:
                    ai += 1
                else:
                    bi += 1
        return res   
    ```

* Construct Binary Search Tree from Preorder Traversal
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


* Uncrossed Lines
    * https://blog.csdn.net/CSerwangjun/article/details/89705729
    ```python
    def maxUncrossedLines(self, A: List[int], B: List[int]) -> int:
        if len(A)<len(B):
            tmp=A
            A=B
            B=tmp
        lenA=len(A)
        lenB=len(B)
        dp=[0]*(lenB+1)
        for i in range(lenA):
            # pay attention to the reversion
            for t in range(lenB)[::-1]:
                if A[i]==B[t]:dp[t+1]=dp[t]+1
            for k in range(lenB):
                dp[k+1]=max(dp[k+1],dp[k])
        return dp[lenB]
    ```
    
* Contiguous Array
    ```python
    def findMaxLength(self, nums: List[int]) -> int:
        total_sum = 0
        index_map = {}
        index_map[0] = -1
        res = 0        
        for i, num in enumerate(nums):
            if num == 0:
                total_sum -= 1
            else:
                total_sum += 1
            if total_sum in index_map:
                res = max(res, i - index_map[total_sum])
            else:
                index_map[total_sum] = i
        return res
    ```
* Possible Bipartition
    ```python
    def possibleBipartition(self, N: int, dislikes: List[List[int]]) -> bool:
        
        if N == 0:
            return True
        
        graph = collections.defaultdict(list)
        
        for dislike in dislikes:
            graph[dislike[0] - 1].append(dislike[1] - 1)
            graph[dislike[1] - 1].append(dislike[0] - 1)
        
        color = [0] * N
        
        for i in range(N):
            if color[i] != 0: continue
            bfs = collections.deque()
            bfs.append(i)
            color[i] = 1
            while bfs:
                cur = bfs.popleft()
                for e in graph[cur]:
                    if color[e] != 0:
                        if color[cur] == color[e]:
                            return False
                    else:
                        color[e] = - color[cur]
                        bfs.append(e)
        return True
    ```
* Counting Bits
    * Method 1
    ```python
    def countBits(self, num: int) -> List[int]:
        res = []
        for i in range(num+1):
            res.append(bin(i).count('1'))
        return res
    ```
    * Method 2
    ```python
    def countBits(self, num: int) -> List[int]:
        dp = [0]
        for i in range(1, num + 1):
            dp.append(dp[i >> 1] + (i & 1))
        return dp
    ```
    * Reference
        * https://blog.csdn.net/coder_orz/article/details/52063216