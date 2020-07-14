---
layout: post
title:  "July LeetCoding Challenge Week 2"
date:   2020-07-14 22:00:30 +0800
categories: Leetcode
tags : Leetcode, Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by July LeetCoding Challenge Week 2.




* Maximum Width of Binary Tree
    * 使用對列
        ```python
        def widthOfBinaryTree(self, root):
            queue = collections.deque()
            queue.append((root, 1))
            res = 0
            while queue:
                width = queue[-1][1] - queue[0][1] + 1
                res = max(width, res)
                for _ in range(len(queue)):
                    n, c = queue.popleft()
                    if n.left: 
                        queue.append((n.left, c * 2))
                    if n.right: 
                        queue.append((n.right, c * 2 + 1))
            return res
        ```

* Flatten a Multilevel Doubly Linked List
    * DFS負責查找，新定義的函數負責插入
        ```python
        class Solution:
            def flatten(self, head: 'Node') -> 'Node':
                if not head: return None
                node = head
                while node:
                    node_next = node.next
                    if node.child:
                        flattened = self.flatten(node.child)
                        node.child = None
                        nextNode = self.appendToList(node, flattened)
                        node = nextNode
                    else:
                        node = node.next
                return head

            def appendToList(self, node, listToAppendHead):
                next_node = node.next
                node.next = listToAppendHead
                listToAppendHead.prev = node
                while node.next:
                    node = node.next
                node.next = next_node
                if next_node:
                    next_node.prev = node
                return next_node
        ```

* Subsets
    ```python
    class Solution:
        def subsets(self, nums: List[int]) -> List[List[int]]:
            res = []
            self.dfs(nums, 0, res, [])
            return res
        def dfs(self, nums, index, res, path):
            res.append(path)
            for i in range(index, len(nums)):
                self.dfs(nums, i + 1, res, path + [nums[i]])
    ```

* Reverse Bits
    * 利用bin()
        ```python
        def reverseBits(self, n: int) -> int:
            b=bin(n)[2:]
            b=(32-len(b))*'0'+b
            b=b[::-1]
            return int(b,2)
        ```
    * 按位處理
        ```python
        def reverseBits(self, n: int) -> int:
            res = 0
            for i in range(32):
                res <<= 1
                res |= ((n >> i) & 1)
            return res
        ```
* Same Tree
    ```python
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        if not p and not q:
            return True
        if not p or not q:
            return False
        if p.val != q.val:
            return False
        left = self.isSameTree(p.left, q.left)
        right = self.isSameTree(p.right, q.right)
        return left and right
    ```
    
* Angle Between Hands of a Clock
    ```python
    def angleClock(self, hour: int, minutes: int) -> float:
        # validate the input 
        if (hour < 0 or minutes < 0 or hour > 12 or minutes > 60): 
            print('Wrong input') 
          
        if (hour == 12): 
            hour = 0
        if (minutes == 60): 
            minutes = 0
            hour += 1
            if(hour > 12): 
                   hour = hour - 12 
          
        # Calculate the angles moved by  
        # hour and minute hands with  
        # reference to 12:00 
        hour_angle = 0.5 * (hour * 60 + minutes) 
        minute_angle = 6 * minutes 
          
        # Find the difference between two angles 
        angle = abs(hour_angle - minute_angle) 
          
        # Return the smaller angle of two  
        # possible angles 
        angle = min(360 - angle, angle) 
          
        return angle 
    ```