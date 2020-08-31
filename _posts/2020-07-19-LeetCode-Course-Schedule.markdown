---
layout: post
title:  "LeetCode Course Schedule"
date:   2020-07-19 09:16:30 +0800
categories: Leetcode
tags : Leetcode Python 
mathjax: true
---
* content 
{:toc}
Topic is Course Schedule and pracices DFS.





## 207 Course Schedule
* Code    
    ```python
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        graph = collections.defaultdict(list)
        for u, v in prerequisites:
            graph[u].append(v)
        # 0 = Unknown, 1 = visiting, 2 = visited
        visited = [0] * numCourses
        def dfs(graph, visited, i):
            if visited[i] == 1: 
                return False
            if visited[i] == 2: 
                return True
            visited[i] = 1
            for j in graph[i]:
                if not dfs(graph, visited, j):
                    return False
            visited[i] = 2
            return True
        for i in range(numCourses):
            if not dfs(graph, visited, i):
                return False
        return True
    ```

## 210 Course Schedule II
* Code
    ```python
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        graph = collections.defaultdict(list)
        for u, v in prerequisites:
            graph[u].append(v)
        # 0 = Unknown, 1 = visiting, 2 = visited
        visited = [0] * numCourses
        def dfs(graph, visited, i, path):
            if visited[i] == 1: return False
            if visited[i] == 2: return True
            visited[i] = 1
            for j in graph[i]:
                if not dfs(graph, visited, j, path):
                    return False
            visited[i] = 2
            path.append(i)
            return True
        path = []
        for i in range(numCourses):
            if not  dfs(graph, visited, i, path):
                return []
        return path
    ```

## 520 Course Schedule III
Greedy Algorithm
* Step
    1. 對所有課程以第二個關鍵詞，進行排序
    2. 依次選擇課程，判斷這個課程添加到課程表裡面是否合適?
        a. 合適，即添加到課程表裡面
        b. 不合適，從課程裡面取出一個課程時間最長的課與當前課程對比，
    3. 依此類推，直到結束
* Code
    ```python
    def scheduleCourse(self, courses: List[List[int]]) -> int:
        totalLen = 0
        heap = []
        for t, d in sorted(courses, key=lambda x: x[1]):
            if totalLen + t <= d:
                totalLen += t
                heapq.heappush(heap, -t)
            elif heap and t < -heap[0]:
                totalLen += t + heapq.heappop(heap)
                heapq.heappush(heap, -t)

        return len(heap)
    ```
## 1462 Course Schedule IV
* Code
    ```python
    class Solution:
        def checkIfPrerequisite(self, n: int, prerequisites: List[List[int]], queries: List[List[int]]) -> List[bool]:
            self.graph = collections.defaultdict(list)
            for pre in prerequisites:
                self.graph[pre[0]].append(pre[1])
            return [self.dfs(query[0], query[1]) for query in queries]

        # start -> end ?
        @functools.lru_cache
        def dfs(self, start, end):
            if start == end:
                return True
            return any(self.dfs(nxt, end) for nxt in self.graph[start])
    ```

## Reference
* https://blog.csdn.net/fuxuemingzhu/article/details/82951771
* https://blog.csdn.net/XX_123_1_RJ/article/details/84862706
* https://blog.csdn.net/fuxuemingzhu/article/details/106464675