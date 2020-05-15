---
layout: post
title:  "May LeetCoding Challenge Week 2"
date:   2020-05-15 09:52:30 +0800
categories: Leetcode
tags : Leetcode, Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by May LeetCoding Challenge Week 2.




* Check If It Is a Straight Line
    ```python
    def checkStraightLine(self, coordinates: List[List[int]]) -> bool:
        if len(coordinates) == 2 : 
            return True
        if coordinates[0][0]!=coordinates[1][0]:
            k = (coordinates[1][1]-coordinates[0][1])/(coordinates[1][0]-coordinates[0][0])
        else:
            k = float('inf')

        for i in range(2, len(coordinates)):
            temp = 0
            if coordinates[i][0]!=coordinates[i-1][0]:
                temp = (coordinates[i][1]-coordinates[i-1][1])/(coordinates[i][0]-coordinates[i-1][0])
            else:
                temp = float('inf')
            if temp!=k:
                return False
        return True
    ```

* Valid Perfect Square
    ```python
    def isPerfectSquare(self, num: int) -> bool:
        if num<=1:
            return True
        x = num
        while True:
            temp = x
            x = (x + num / x ) / 2
            if abs(x - temp) < 1:
                break
        if int(x)*int(x) == num:
            return True
        else:
            return False
    ```

* Find the Town Judge
    ```python
    def findJudge(self, N: int, trust: List[List[int]]) -> int:
        graph = {i:[] for i in range(1, N+1)}
        for t in trust:
            graph[t[0]].append(t[1])

        # check if the judge exists
        for k in graph:
            if len(graph[k]) == 0:
                # check if others trusts the judge
                judge = k
                for person in graph:
                    if person != judge and judge not in graph[person]:
                        return -1
                return judge

        return -1
    ```
    
* Flood Fill
    * DFS
        ```python
        def floodFill(self, image: List[List[int]], sr: int, sc: int, newColor: int) -> List[List[int]]:
            SR, SC = len(image), len(image[0])
            color = image[sr][sc]
            if color == newColor:
                return image
            def dfs(r, c):
                if image[r][c] == color:
                    image[r][c] = newColor
                    if r >= 1:
                        dfs(r-1, c)
                    if r < SR-1:
                        dfs(r+1, c)
                    if c >=1:
                        dfs(r, c-1)
                    if c < SC-1:
                        dfs(r, c+1)
            dfs(sr,sc)
            return image
        ```
    * BFS
        ```python
        def floodFill(self, image: List[List[int]], sr: int, sc: int, newColor: int) -> List[List[int]]:
            que = collections.deque()
            que.append((sr, sc))
            start = image[sr][sc]
            if start == newColor: return image
            M, N = len(image), len(image[0])
            directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]
            while que:
                pos = que.popleft()
                image[pos[0]][pos[1]] = newColor
                for d in directions:
                    newx, newy = pos[0] + d[0], pos[1] + d[1]
                    if 0 <= newx < M and 0 <= newy < N and image[newx][newy] == start:
                        que.append((newx, newy))
            return image
        ```
        
* Single Element in a Sorted Array
    * O(n) : 72ms
        ```python
        def singleNonDuplicate(self, nums: List[int]) -> int:
            ans = 0
            for n in nums:
                ans ^= n
            return ans
        ```
    * O(log n) : 68ms
        ```python
        def singleNonDuplicate(self, nums: List[int]) -> int:
            low = 0
            high = len(nums) - 1
            while True:
                mid = (low + high) // 2
                if mid % 2 == 1:
                    mid -= 1
                if mid < high and nums[mid] == nums[mid+1]:
                    low = mid + 2
                elif mid > low and nums[mid] == nums[mid-1]:
                    high = mid - 2
                else:
                    return nums[mid]
       ```
     * 52ms
        ```python
         def singleNonDuplicate(self, nums: List[int]) -> int:
            return sum(set(nums))*2-sum(nums)
        ```
* Remove K Digits
    ```python
    def removeKdigits(self, num: str, k: int) -> str:
        if len(num) == k:
            return '0'
        stack = []
        for n in num:
            while stack and k and int(stack[-1]) > int(n):
                stack.pop()
                k -= 1
            stack.append(n)
        while k:
            stack.pop()
            k -= 1
        if not stack:
            return '0'
        return str(int(''.join(stack)))
    ```
    
* Implement Trie(Prefix Tree)
    ```python
    class Node(object):
        def __init__(self):
            self.children = collections.defaultdict(Node)
            self.isword = False
    class Trie:

        def __init__(self):
            """
            Initialize your data structure here.
            """
            self.root = Node()


        def insert(self, word: str) -> None:
            """
            Inserts a word into the trie.
            """
            current = self.root
            for w in word:
                current = current.children[w]
            current.isword = True

        def search(self, word: str) -> bool:
            """
            Returns if the word is in the trie.
            """
            current = self.root
            for w in word:
                current = current.children.get(w)
                if current == None:
                    return False
            return current.isword


        def startsWith(self, prefix: str) -> bool:
            """
            Returns if there is any word in the trie that starts with the given prefix.
            """
            current = self.root
            for w in prefix:
                current = current.children.get(w)
                if current == None:
                    return False
            return True
    ```