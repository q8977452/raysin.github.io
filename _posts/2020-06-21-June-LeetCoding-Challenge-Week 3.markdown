---
layout: post
title:  "June LeetCoding Challenge Week 3"
date:   2020-06-21 22:00:30 +0800
categories: Leetcode
tags : Leetcode, Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by June LeetCoding Challenge Week 3.




* Search in a Binary Search Tree
    * Recursive method
    ```python
    def searchBST(self, root: TreeNode, val: int) -> TreeNode:
        if not root:
            return None
        if root.val == val:
            return root
        elif root.val < val:
            return self.searchBST(root.right, val)
        else:
            return self.searchBST(root.left, val)
    ```
    
* Validate IP Address
    ```python
    def validIPAddress(self, IP: str) -> str:
        def isIPv4(s):
            try : 
                return str(int(s)) == s and 0 <= int(s) <=255
            except:
                return False
        def isIPv6(s):
            if len(s) > 4:
                return False
            try:
                return int(s, 16) >= 0 and s[0] != '-'
            except:
                return False
        if IP.count(".") == 3 and all(isIPv4(i) for i in IP.split(".")): 
            return "IPv4"
        if IP.count(":") == 7 and all(isIPv6(i) for i in IP.split(":")): 
            return "IPv6"
        return "Neither"
    ```
* Surrounded Regions
    * https://blog.csdn.net/qqxx6661/article/details/78546695
    ```python
    def solve(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        if not board or not board[0]:
            return
        n = len(board)
        m = len(board[0])
        queue = []
        # Save all 0 of edges
        for i in range(n):
            for j in range(m):
                if ((i in (0, n - 1)) or (j in (0, m - 1))) and board[i][j] == 'O':  
                    queue.append((i, j))
                    print (i,j)
        # Save all 0 associated the 0 of edges
        while queue:
            r, c = queue.pop(0)  # i, j = r, c
            if 0 <= r < n and 0 <= c < m and board[r][c] == 'O':
                board[r][c] = 'M'  # M is not necessary for x 
                if r - 1 >= 0 and board[r - 1][c] == 'O':
                    queue.append((r - 1, c))
                if r + 1 < n and board[r + 1][c] == 'O':
                    queue.append((r + 1, c))
                if c - 1 >= 0 and board[r][c - 1] == 'O':
                    queue.append((r, c - 1))
                if c + 1 < m and board[r][c + 1] == 'O':
                    queue.append((r, c + 1))
        # Update
        for i in range(n):
            for j in range(m):
                if board[i][j] == 'M':
                    board[i][j] = 'O'
                else:
                    board[i][j] = 'X'
    ```
    
* H-Index II
    ```python
    def hIndex(self, citations: List[int]) -> int:
            N = len(citations)
            l, r = 0, N - 1
            H = 0
            while l <= r:
                mid = int(l + (r - l) / 2)
                H = max(H, min(citations[mid], N - mid))
                if citations[mid] < N - mid:
                    l = mid + 1
                else:
                    r = mid - 1
            return H
    ```
    
* Longest Duplicate Substring
    ```python
    def longestDupSubstring(self, S: str) -> str:    
        mod=2**63-1
        
        def hash(s):
            ss=[ord(c)-ord('a') for c in s]
            res=0
            for c in ss: 
                res*=26
                res+=c
                res%=mod
            return res
        
        def hash2(prev,pre_char,cur_char,const):
            return ((prev-const*(ord(pre_char)-ord('a')))*26+(ord(cur_char)-ord('a')))%mod
            
        def ok(mid):
            const=(26**(mid-1))%mod
            cur_hash = hash(S[:mid])
            s=set()
            s.add(cur_hash)
            for i in range(1,len(S)-mid+1):
                sub_hash = hash2(cur_hash,S[i-1],S[i+mid-1],const)
                cur_hash = sub_hash
                if sub_hash in s: return True,S[i:i+mid]
                s.add(sub_hash)
            return False,''
        
        lo,hi=1,len(S)-1
        res=''
        while lo<hi:
            mid=(lo+hi)//2
            f,r=ok(mid)
            if f:
                res=r
                lo=mid+1
            else:
                hi=mid-1
        f,r=ok((lo+hi)//2)
        if f: 
            res=r
        return res
    ```

* Permutation Sequence
    ```python
    def getPermutation(self, n: int, k: int) -> str:
        ans = ''
        fact = [1] * n
        num = [str(i) for i in range(1, 10)]
        for i in range(1, n):
            fact[i] = fact[i - 1] * i
        k -= 1
        for i in range(n, 0, -1):
            first = k // fact[i - 1]
            k %= fact[i - 1]
            ans += num[first]
            num.pop(first)
        return ans
    ```

* Dungeon Game
    ```python
    def calculateMinimumHP(self, dungeon: List[List[int]]) -> int:
        if len(dungeon) == 0:
            return None
        row = len(dungeon)
        col = len(dungeon[0])
        dp = [[0 for _ in range(col)] for _ in range(row) ] 
        dp[row-1][col-1] = max(1,-dungeon[-1][-1] + 1)
        for i in range(row-1,-1,-1):
            for j in range(col - 1,-1,-1):
                down = None
                if i + 1 < row:
                    down = max(1,dp[i+1][j] - dungeon[i][j])
                right = None
                if j + 1 < col:
                    right = max(1,dp[i][j+1] - dungeon[i][j])
                if down and right:
                    dp[i][j] = min(down,right)
                elif down:
                    dp[i][j] = down
                elif right:
                    dp[i][j] = right
        return dp[0][0]
    ```