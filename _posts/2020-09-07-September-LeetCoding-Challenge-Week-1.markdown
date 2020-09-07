---
layout: post
title:  "September LeetCoding Challenge Week 1"
date:   2020-09-07 20:45:30 +0800
categories: Leetcode
tags : Leetcode Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by September LeetCoding Challenge Week 1.




* Largest Time for Given Digits
    ```python
    def largestTimeFromDigits(self, A: List[int]) -> str:
        if(sum(A)==0):
            return "00:00"
        li=["0","0"]
        for i in range(4):
            for j in range(4):
                for k in range(4):
                    for q in range(4):
                        if(i!=j and i!=k and i!=q and j!=k and j!=q and k!=q):
                            h=str(A[i])+str(A[j])
                            m=str(A[k])+str(A[q])
                            if(int(h)<24 and int(m) < 60):
                                if(int(h)>int(li[0]) or (int(h)==int(li[0]) and int(m) > int(li[1]))):
                                    li=[h,m]
        if((int(li[0])+int(li[1]))==0):
            return ""
        else:
            return li[0]+":"+li[1]
    ```
    
* Contains Duplicate III
    ```python
    def containsNearbyAlmostDuplicate(self, nums: List[int], k: int, t: int) -> bool:
        if k < 1 or t < 0:
            return False
        dic = collections.OrderedDict()
        for n in nums:
            key = n if not t else n // t
            for m in (dic.get(key - 1), dic.get(key), dic.get(key + 1)):
                if m is not None and abs(n - m) <= t:
                    return True
            if len(dic) == k:
                dic.popitem(False)
            dic[key] = n
        return False
    ```
    
* Repeated Substring Pattern python
    ```python
    def repeatedSubstringPattern(self, s: str) -> bool:
        len_s = len(s)
        for i in range(1, len_s // 2 + 1):
            if len_s % i == 0:
                sub_s = s[:i]
                if sub_s * (len_s // i) == s:
                    return True
        return False
    ```
    
* Partition Labels
    ```python
    def partitionLabels(self, S: str) -> List[int]:
        lindex = {c: i for i, c in enumerate(S)}
        j = anchor = 0
        ans = []
        for i, c in enumerate(S):
            j = max(j, lindex[c])
            if i == j:
                ans.append(j - anchor + 1)
                anchor = j + 1
        return ans
    ```
    
* All Elements in Two Binary Search Trees
    ```python
    def getAllElements(self, root1: TreeNode, root2: TreeNode) -> List[int]:
        def dfs(root, arr):
            if not root:
                return 
            dfs(root.left, arr)
            arr.append(root.val)
            dfs(root.right, arr)
        
        list1, list2 = [], []
        dfs(root1, list1)
        dfs(root2, list2)
        list1.extend(list2)
        list1.sort()
        return list1
    ```
    
*  Image Overlap
    ```python
    def largestOverlap(self, A: List[List[int]], B: List[List[int]]) -> int:
        N = len(A)
        LA = [(xi, yi) for xi in range(N) for yi in range(N) if A[xi][yi]]
        LB = [(xi, yi) for xi in range(N) for yi in range(N) if B[xi][yi]]
        d = collections.Counter([(x1 - x2, y1 - y2) for (x1, y1) in LA for (x2, y2) in LB])
        return max(d.values() or [0])
    ```
    
* Word Pattern
    ```python
    def wordPattern(self, pattern: str, str: str) -> bool:
        strs = str.split()
        if len(pattern) != len(strs):
            return False
        d = dict()
        for i, p in enumerate(pattern):
            if p not in d:
                d[p] = strs[i]
            else:
                if d[p] != strs[i]:
                    return False
        d = dict()
        for i, p in enumerate(strs):
            if p not in d:
                d[p] = pattern[i]
            else:
                if d[p] != pattern[i]:
                    return False
        return True
    ```