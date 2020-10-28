---
layout: post
title:  "October LeetCoding Challenge Week 4"
date:   2020-10-28 18:04:30 +0800
categories: Leetcode
tags : Leetcode Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by October LeetCoding Challenge Week 4.




* Minimum Depth of Binary Tree
    * 分析/觀念
        * 深度是從根走到這個節點的長度
        * 最小的路徑長度，可以選擇的方式就是分別看兩邊有多深，最後選擇較小的那邊，加上1即可
    * 程式
        ```python
        def minDepth(self, root: TreeNode) -> int:
            if not root:
                return 0
            if not root.left or not root.right:
                return max(self.minDepth(root.left), self.minDepth(root.right)) + 1
            else:
                return min(self.minDepth(root.left), self.minDepth(root.right)) + 1
        ```
        
* 132 Pattern
    * 維護一個棧和一個變量third，其中third就是第三個數字，也是pattern
        * 132中的2，棧裡面按順序放所有大於third的數字，也是pattern
        * 132中的3，那麼我們在遍歷的時候，如果當前數字小於third，即pattern
        * 132中的1找到了，我們直接返回true即可，因為已經找到了，注意我們應該從後往前遍歷數組
    * 如果當前數字大於棧頂元素，那麼我們按順序將棧頂數字取出，賦值給third，然後將該數字壓入棧，這樣保證了棧裡的元素仍然都是大於third的，我們想要的順序依舊存在，進一步來說，棧裡存放的都是可以維持second > third的second值，其中的任何一個值都是大於當前的third值
    * 如果有更大的值進來，那就等於形成了一個更優的second > third的這樣一個組合，並且這時彈出的third值比以前的third值更大，為什麼要保證third值更大，因為這樣才可以更容易的滿足當前的值first比third值小這個條件。
    * 程式
        ```python
         def find132pattern(self, nums: List[int]) -> bool:
            if len(nums) <=2:
                return False
            third = float('-inf')
            stack = []
            for i in range(len(nums)-1, -1, -1):
                if nums[i] < third:
                    return True
                else:
                    while stack and stack[-1] < nums[i]:
                        third = stack.pop()
                stack.append(nums[i])
            return False
        ```
    * Reference
        * https://www.cnblogs.com/grandyang/p/6081984.html


* Bag of Tokens
    * 分析
        * 步驟
            * 對token排序
            * 第一步
                * 用現在所有的power把左邊的都翻成正面，同時獲得了一些點
                    * 這一步之後，我們使用power貪心地獲得了所有的點數
            * 第二步
                * 看右邊能翻轉多少個反面，這個能獲得Power
                    * 使用的點數換取了更多的power
            * 這兩步來回走的話，就能獲得更多的Points
        * 需要注意
            * 如果剩餘的token只有一個的時候，我們是不把它換成power
    * 程式
        ```python
        def bagOfTokensScore(self, tokens: List[int], P: int) -> int:
            tokens.sort()
            N = len(tokens)
            left, right = 0, N - 1
            points = 0
            remain = N
            while left < N and P >= tokens[left]:
                P -= tokens[left]
                points += 1
                left += 1
                remain -= 1
            if left == 0 or left == N: return points
            while points > 0 and remain > 1:
                P += tokens[right]
                right -= 1
                points -= 1
                remain -= 1
                while left <= right and P >= tokens[left]:
                    P -= tokens[left]
                    points += 1
                    left += 1
                    remain -= 1
            return points
        ```
        
*  Stone Game IV
    *  想法
        *  Let win(n) denotes whether the current play will win or not.
        *  Try all possible square numbers and see whether the other player will lose or not.
        *  win(n) = any(win(n – i*i) == False) ? True : False 
        *  base case
            *  win(0) = False
    * 程式
        ```python
        def winnerSquareGame(self, n: int) -> bool:
            dp = [None] * (n + 1)
            dp[0] = False
            for i in range(0, n):      
                if dp[i]: continue
                for j in range(1, n + 1):      
                    if i + j * j > n: break
                    dp[i + j * j] = True
            return dp[n]
        ```
        
* Champagne Tower
    ```python
    def champagneTower(self, poured: int, query_row: int, query_glass: int) -> float:
        N = 100
        dp = [[0] * N for _ in range(N)]
        dp[0][0] = poured
        for i in range(query_row):
            for j in range(i + 1):
                if dp[i][j] > 1:
                    dp[i + 1][j    ] += (dp[i][j] - 1) / 2.0
                    dp[i + 1][j + 1] += (dp[i][j] - 1) / 2.0
        return min(1, dp[query_row][query_glass])
    ```
    
* Linked List Cycle II
    ```python
    def detectCycle(self, head: ListNode) -> ListNode:
        if not head: return None
        visited = set()
        while head:
            if head in visited:
                return head
            visited.add(head)
            head = head.next
        return None
    ```
    
* Summary Ranges
    * 分析
        * 兩層While
            * 第一層跑Nums
            * 第二層往後走，看看後面的數字是不是比前面的數字大1，是的話就一直後移
                * 根據i和j是否重疊來判斷
                    * 是加上一個數字
                    * 還是加上一個區間
    * 程式碼
        ```python
        def summaryRanges(self, nums: List[int]) -> List[str]:
            if not nums: return []
            res = []
            i = 0
            while i < len(nums):
                j = i
                while j < len(nums) - 1 and nums[j] == nums[j + 1] - 1:
                    j += 1
                if j == i:
                    res.append(str(nums[i]))
                else:
                    res.append('%s->%s' % (str(nums[i]), str(nums[j])))
                i = j + 1
            return res
        ```