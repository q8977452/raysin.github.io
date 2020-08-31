---
layout: post
title:  "August LeetCoding Challenge Week 3"
date:   2020-08-21 23:51:30 +0800
categories: Leetcode
tags : Leetcode Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by August LeetCoding Challenge Week 3.





* Find Permutation
    ```python
    def findPermutation(self, s):
        """
        :type s: str
        :rtype: List[int]
        """
        res = []
        cur = 1
        i = 0
        while i<len(s):
            if i==0:
                if s[i]=='I':
                    cur = 1
                    res += [cur]
                else:
                    cur = 0
                    # res += [cur]
            # j = (s[j]=='D')?i:(i+1)
            
            if s[i]== 'D':
                j = i
            else:
                j = i+1
            cntD = 0
            while j<len(s) and s[j]=='D':
                j += 1
                cntD += 1
            # cntD = j-i
            cur =  cntD + cur + 1
            # print(cur)
            res += [cur]         
            for h in range(cntD):
                res += [cur- 1 - h]
            i = j
        return res 
    ```
* Non-overlapping Intervals
    ```python
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        if not intervals: return 0
        intervals.sort(key = lambda x : x[0])
        last = 0
        res = 0
        for i in range(1, len(intervals)):
            if intervals[last][1] > intervals[i][0]:
                if intervals[i][1] < intervals[last][1]:
                    last = i
                res += 1
            else:
                last = i
        return res
    ```
    
* Best Time to Buy and Sell Stock III
    ```python
    def maxProfit(self, prices: List[int]) -> int:
        if not prices: return 0
        N = len(prices)
        g = [[0] * 3 for _ in range(N)]
        l = [[0] * 3 for _ in range(N)]
        for i in range(1, N):
            diff = prices[i] - prices[i - 1]
            for j in range(1, 3):
                l[i][j] = max(g[i - 1][j - 1] + max(diff, 0), l[i - 1][j] + diff)
                g[i][j] = max(l[i][j], g[i - 1][j])
        return g[-1][-1]
    ```
    
* Distribute Candies to People
    ```python
    def distributeCandies(self, candies: int, num_people: int) -> List[int]:
        ans = [0] * num_people
        give = 0
        while candies > 0:
            ans[give % num_people] += min(give + 1, candies)
            give += 1
            candies -= give
        return ans
    ```
    
* Numbers With Same Consecutive Differences
    * Self
        ```python
        class Solution:
            def numsSameConsecDiff(self, N: int, K: int) -> List[int]:
                if N == 1:
                    return [0, 1,2,3,4,5,6,7,8,9]
                res = []
                for i in range(1, 10):
                    self.dfs(res, i, N - 1, K)
                return list(set(res))

            def dfs(self, res, curint, N, K):
                if N == 0:
                    res.append(curint)
                    return
                last = curint % 10
                if last + K <= 9:
                    self.dfs(res, curint * 10 + last + K, N - 1, K)
                if last - K >= 0:
                    self.dfs(res, curint * 10 + last - K, N - 1, K)
        ```
    * No Self
        ```python
        def numsSameConsecDiff(self, N: int, K: int) -> List[int]:
            if N == 1:
                return [0, 1,2,3,4,5,6,7,8,9]
            res = []
            def dfs(res, curint, N, K):
                if N == 0:
                    res.append(curint)
                    return
                last = curint % 10
                if last + K <= 9:
                    dfs(res, curint * 10 + last + K, N - 1, K)
                if last - K >= 0:
                    dfs(res, curint * 10 + last - K, N - 1, K)
            for i in range(1, 10):
                dfs(res, i, N - 1, K)
            return list(set(res))
        ```
        
* Goat Latin
    ```python
    def toGoatLatin(self, S: str) -> str:
        vowels = ['a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U']
        words = S.split(' ')
        new_words = []
        for i, word in enumerate(words):
            if word[0] in vowels:
                word += 'ma'
            else:
                word = word[1:] + word[0] + 'ma'
            word += 'a' * (i + 1)
            new_words.append(word)
        return ' '.join(new_words)
    ```
    
* Reorder List
    ```python
    def reorderList(self, head: ListNode) -> None:
        """
        Do not return anything, modify head in-place instead.
        """
        if head and head.next and head.next.next:
            #find mid
            fast, slow = head, head
            while fast.next and fast.next.next:
                fast = fast.next.next
                slow = slow.next
            head1 = head
            head2 = slow.next
            slow.next = None

            # reverse linked list head2     
            dummy = ListNode(0)
            dummy.next = head2
            p = head2.next
            head2.next = None
            while p:
                temp = p
                p = p.next
                temp.next = dummy.next
                dummy.next = temp
            head2 = dummy.next

            # merge two linked list head1 and head2
            p1 = head1
            p2 = head2
            while p2:
                temp1 = p1.next
                temp2 = p2.next
                p1.next = p2
                p2.next = temp1
                p1 = temp1
                p2 = temp2
    ```
    
* Sort Array By Parity
    ```python
    def sortArrayByParity(self, A: List[int]) -> List[int]:
        return sorted(A, key = lambda x : x % 2)
    ```