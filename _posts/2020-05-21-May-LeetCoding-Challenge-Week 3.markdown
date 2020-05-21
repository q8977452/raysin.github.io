---
layout: post
title:  "May LeetCoding Challenge Week 3"
date:   2020-05-21 22:44:30 +0800
categories: Leetcode
tags : Leetcode, Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by May LeetCoding Challenge Week 3.




* Maximum Sum Circular Subarray
    * maximum sum is either max (we get using kadane's algo) or (sum - min).There is a special case, if sum == min,then maximum sum is max.
    * Let's develop an algorithm to solve this problem.
        1. Find maximum subarray sum using kadane's algorithm (max) 
        2. Find minimum subarray sum using kadane's algorithm (min)
        3. Find total sum of the array (sum)
        4. Now, if sum == min return max
        5. Otherwise, return maximum ( max, sum - min )
    * In Practice
	```python
    def maxSubarraySumCircular(self, A: List[int]) -> int:
        if len(A) == 0:
            return 0
        maxTotal,maxSoFar,minSoFar,minTotal,s = A[0], A[0], A[0], A[0],A[0]
        for i in range(1, len(A)):
            maxSoFar = max(A[i], maxSoFar + A[i])
            maxTotal = max(maxTotal, maxSoFar)            
            
            minSoFar = min(A[i], minSoFar + A[i])            
            minTotal = min(minTotal, minSoFar)            
            s += A[i]
        if(s == minTotal):
            return maxTotal
        
        return max(s - minTotal, maxTotal)
    ```

* Odd Even Linked List
    ```python
    def oddEvenList(self, head: ListNode) -> ListNode:
        odd = ListNode(0)
        even = ListNode(0)
        oddHead, evenHead = odd, even
        index = 0
        while head:
            if index & 1 == 0:
                odd.next = head
                odd = odd.next
            else:
                even.next = head
                even = even.next
            head = head.next
            index += 1
        even.next = None
        odd.next = evenHead.next
        return oddHead.next
    ```

* Find All Anagrams in a String
    * Sliding Window
    ```python
    def findAnagrams(self, s: str, p: str) -> List[int]:
        from collections import Counter
        answer = []
        m, n = len(s), len(p)
        if m < n:
            return answer
        pCounter = Counter(p)
        sCounter = Counter(s[:n-1])
        index = 0
        for index in range(n-1, m):
            sCounter[s[index]] += 1
            if sCounter == pCounter:
                answer.append(index - n + 1)
            sCounter[s[index - n + 1]] -= 1
            if sCounter[s[index - n + 1]] == 0:
                del sCounter[s[index - n + 1]]
        return answer
    ```
    * Two Pointers
    ```python
    def findAnagrams(self, s: str, p: str) -> List[int]:
        from collections import Counter
        count = Counter()
        m, n = len(s), len(p)
        left, right = 0, 0
        pcount = Counter(p)
        res = []
        while right < m:
            count[s[right]] += 1
            if right - left + 1 == n:
                if count == pcount:
                    res.append(left)
                count[s[left]] -= 1
                if count[s[left]] == 0:
                    del count[s[left]]
                left += 1
            right += 1
        return res
    ```
    
* Permutation in String
    * Time Limit Exceeded
    ```python
    def checkInclusion(self, s1: str, s2: str) -> bool:
        from itertools import permutations
        perms = [''.join(p) for p in permutations(s1)]
        m = -2
        for x in perms:
            m = max(m, s2.find(x))
        return True if m >-1 else False
    ```
    * Sliding Window
    ```python
    def checkInclusion(self, s1: str, s2: str) -> bool:
        from collections import Counter
        if len(s2) < len(s1): 
            return False
        c = Counter(s1)
        n = len(s1)
        l, r = 0, n - 1
        s = Counter(s2[l : r])
        while r < len(s2):
            s[s2[r]] += 1
            if s == c:
                return True
            s[s2[l]] -= 1
            if s[s2[l]] == 0:
                del s[s2[l]]
            l += 1
            r += 1
        return False
    ```

* Online Stock Span
    * Monotone Decreasing Stack
    ```python
    class StockSpanner:

        def __init__(self):
            self.ans = []

        def next(self, price: int) -> int:
            res = 1
            while self.ans and self.ans[-1][0] <=price:
                res += self.ans.pop()[1]
            self.ans.append((price,res))
            return res
    ```
    
* Kth Smallest Element in a BST
    ```python
    # Definition for a binary tree node.
    class TreeNode:
        def __init__(self, val=0, left=None, right=None):
            self.val = val
            self.left = left
            self.right = right
    class Solution:
        def kthSmallest(self, root: TreeNode, k: int) -> int:
            stack = []
            node = root
            while node:
                stack.append(node)
                node = node.left
            x = 1
            while stack and x <= k:
                node = stack.pop()
                x += 1
                right = node.right
                while right:
                    stack.append(right)
                    right = right.left
            return node.val
    ```
* Count Square Submatrices with All Ones
    * https://blog.csdn.net/XX_123_1_RJ/article/details/104131436
    ```python
    def countSquares(self, matrix: List[List[int]]) -> int:
        for i in range(1, len(matrix)):
            for j in range(1, len(matrix[0])):
                matrix[i][j] *= min(matrix[i-1][j], 
                                    matrix[i][j-1], 
                                    matrix[i-1][j-1]) +1 
        return sum(map(sum, matrix))
    ```