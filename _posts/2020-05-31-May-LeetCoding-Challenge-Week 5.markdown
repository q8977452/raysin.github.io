---
layout: post
title:  "May LeetCoding Challenge Week 5"
date:   2020-05-31 21:52:30 +0800
categories: Leetcode
tags : Leetcode Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by May LeetCoding Challenge Week 5.





* Course Schedule
    * BFS
        ```python
        def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
            graph = collections.defaultdict(list)
            indegrees = collections.defaultdict(int)
            for u, v in prerequisites:
                graph[v].append(u)
                indegrees[u] += 1
            for i in range(numCourses):
                zeroDegree = False
                for j in range(numCourses):
                    if indegrees[j] == 0:
                        zeroDegree = True
                        break
                if not zeroDegree: 
                    return False
                indegrees[j] = -1
                for node in graph[j]:
                    indegrees[node] -= 1
            return True                
        ```
    
    * DFS
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

* K Closet Points to Origin
    * method 1
    ```python
    def kClosest(self, points: List[List[int]], K: int) -> List[List[int]]:
        dis = []
            for p in points:
                d = math.sqrt(p[0] ** 2 + p[1] ** 2)
                dis.append((d,p))
        heapq.heapify(dis)
        return [d[1] for d in heapq.nsmallest(K, dis)]
    ```
    * method 2
    ```python
    def kClosest(self, points: List[List[int]], K: int) -> List[List[int]]:
        def func(x):
            return math.sqrt((x[0]*x[0])+(x[1]*x[1]))
        
        return sorted(points, key=func)[:K]
    ```

* Edit Distance
    ```python
    def minDistance(self, word1: str, word2: str) -> int:
        L1, L2 = len(word1), len(word2)
        dp = [[0] * (L2 + 1) for _ in range(L1 + 1)]
        for i in range(L1 + 1):
            dp[i][0] = i
        for j in range(L2 + 1):
            dp[0][j] = j
        for i in range(1, L1 + 1):
            for j in range(1, L2 + 1):
                if word1[i - 1] == word2[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1]
                else:
                     dp[i][j] = min(dp[i - 1][j - 1], dp[i - 1][j], dp[i][j - 1]) + 1
        return dp[L1][L2]
    ```