---
layout: post
title:  "July LeetCoding Challenge Week 3"
date:   2020-07-21 22:00:30 +0800
categories: Leetcode
tags : Leetcode, Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by July LeetCoding Challenge Week 3.





* Reverse Words in a String
    ```python
    def reverseWords(self, s: str) -> str:
        return " ".join(s.split()[::-1])
    ```

* Pow(x, n)
    * Method : 二分求冪
    ![](https://img-my.csdn.net/uploads/201303/30/1364644733_9955.jpg)
    
    * Code
        ```python
        def myPow(self, x: float, n: int) -> float:
            if n == 0:
                return 1
            if n < 0:
                x = 1 / x
                n = -n
            ans = 1
            res = 1
            while n:
                if n % 2:
                    ans *= x
                n >>= 1
                x *= x
            return ans

        ```
    * Reference 
    [CSDN : 二分求幂（pow的logn算法）](https://blog.csdn.net/prstaxy/article/details/8740838)
    
* Top K Frequent Elements
    ```python
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        counter = Counter(nums).most_common()
        return [counter[i][0] for i in range(k)]
    ```
    
*  Course Schedule II
    ```python
    class Solution:
        def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
            graph = collections.defaultdict(list)
            for u, v in prerequisites:
                graph[u].append(v)
            # 0 = Unknown, 1 = visiting, 2 = visited
            visited = [0] * numCourses
            path = []
            for i in range(numCourses):
                if not self.dfs(graph, visited, i, path):
                    return []
            return path

        def dfs(self, graph, visited, i, path):
            if visited[i] == 1: return False
            if visited[i] == 2: return True
            visited[i] = 1
            for j in graph[i]:
                if not self.dfs(graph, visited, j, path):
                    return False
            visited[i] = 2
            path.append(i)
            return True
    ```
    
* Add Binary
    ```python
    def addBinary(self, a: str, b: str) -> str:
        c=int(a,2)+int(b,2)
        x=bin(c).split('b')

        return x[1]
    ```
    
* Remove Linked List Elements
    ```python
    def removeElements(self, head: ListNode, val: int) -> ListNode:
        new_head = ListNode(0)
        new_head.next = head
        slow, fast = new_head, head
        while fast:
            if fast.val != val:
                slow.next.val = fast.val
                slow = slow.next
            fast = fast.next
        slow.next = None
        return new_head.next
    ```
    
* Word Search
    * 回朔法
        ```python
        class Solution:
            def exist(self, board: List[List[str]], word: str) -> bool:
                for y in range(len(board)):
                    for x in range(len(board[0])):
                        if self.exit(board, word, x, y, 0):
                            return True
                return False

            def exit(self, board, word, x, y, i):
                if i == len(word):
                    return True
                if x < 0 or x >= len(board[0]) or y < 0 or y >= len(board):
                    return False
                if board[y][x] != word[i]:
                    return False
                board[y][x] = board[y][x].swapcase()
                isexit =  self.exit(board, word, x + 1, y, i + 1) or self.exit(board, word, x, y + 1, i + 1) or self.exit(board, word, x - 1, y, i + 1) or self.exit(board, word, x, y - 1, i + 1)
                board[y][x] = board[y][x].swapcase()
                return isexit

        ```