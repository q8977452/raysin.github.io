---
layout: post
title:  "August LeetCoding Challenge Week 4"
date:   2020-08-30 19:00:30 +0800
categories: Leetcode
tags : Leetcode Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by August LeetCoding Challenge Week 4.




* Random Point in Non-overlapping Rectangles
    ```python
    class Solution:

        def __init__(self, rects):
            """
            :type rects: List[List[int]]
            """
            self.rects = rects
            self.N = len(rects)
            areas = [(x2 - x1 + 1) * (y2 - y1 + 1) for x1, y1, x2, y2 in rects]
            self.preSum = [0] * self.N
            self.preSum[0] = areas[0]
            for i in range(1, self.N):
                self.preSum[i] = self.preSum[i - 1] + areas[i]
            self.total = self.preSum[-1]

        def pickRect(self):
            rand = random.randint(0, self.total - 1)
            return bisect.bisect_right(self.preSum, rand)

        def pickPoint(self, rect):
            x1, y1, x2, y2 = rect
            x = random.randint(x1, x2)
            y = random.randint(y1, y2)
            return x, y

        def pick(self):
            """
            :rtype: List[int]
            """
            rectIndex = self.pickRect()
            rect = self.rects[rectIndex]
            return self.pickPoint(rect)
    ```
    
* Stream of Characters
    ```python
    class StreamChecker:

        def __init__(self, words: List[str]):
            self.Trie={}
            for word in words:
                curnode=self.Trie
                word=word[::-1]
                for ch in word:
                    if ch not in curnode:
                        curnode[ch]={}
                    curnode=curnode[ch]
                curnode['#']=1 # '#' means the end of a word
            self.pre=''

        def query(self, letter: str) -> bool:
            self.pre=self.pre+letter
            curnode=self.Trie
            for i in range(0,len(self.pre)):
                if self.pre[-i-1] not in curnode:
                    break
                curnode=curnode[self.pre[-i-1]]
                if '#' in curnode:
                    return True
            return False
    ```
    
* Sum of Left Leaves
    ```python
    def sumOfLeftLeaves(self, root: TreeNode) -> int:
        if not root: return 0
            stack = []
            stack.append(root)
            leftsum = 0
            while stack:
                node = stack.pop()
                if not node: continue
                if node.left:
                    if not node.left.left and not node.left.right:
                        leftsum += node.left.val
                    stack.append(node.left)
                if node.right:
                    stack.append(node.right)
            return leftsum
    ```
    
* Minimum Cost For Tickets
    ```python
    def mincostTickets(self, days: List[int], costs: List[int]) -> int:
        dp = [float("inf")] * 366
        for day in days:
            dp[day] = 0
        dp[0] = 0
        for i in range(1, 366):
            if dp[i] == float("inf"):
                dp[i] = dp[i - 1]
            else:
                cur = dp[i - 1] + costs[0]
                cur = min(dp[max(0, i - 7)] + costs[1], cur)
                cur = min(dp[max(0, i - 30)] + costs[2], cur)
                dp[i] = cur
        return dp[days[-1]]
    ```
    
* Fizz Buzz
    ```python
    def fizzBuzz(self, n: int) -> List[str]:
        res = []
        strmap = {3 : "Fizz", 5 : "Buzz"}
        for i in range(1, n + 1):
            pos = ""
            for j in [3, 5]:
                if i % j == 0:
                    pos += strmap[j]
            if not pos:
                pos = str(i)
            res.append(pos)
        return res
    ```
    
* Find Right Interval
    ```python
    class Solution:
        def findRightInterval(self, intervals: List[List[int]]) -> List[int]:
            n = len(intervals)
            start_map = {interval[0] : i for i, interval in enumerate(intervals)}
            start_list = [interval[0] for interval in intervals]
            res = []
            start_list.sort()
            for interval in intervals:
                pos = self.higher_find(start_list, interval[-1])
                res.append(start_map[start_list[pos]] if pos != -1 else -1)
            return res

        def higher_find(self, array, v):
            lo, hi = 0, len(array) - 1
            first = -1
            while lo <= hi:
                mid = lo + (hi - lo) // 2
                if array[mid] >= v:
                    hi = mid - 1
                    first = mid
                else:
                    lo = mid + 1
            return first
    ```
* Implement Rand10() Using Rand7()
    ```python
    class Solution:
        def rand10(self):
            """
            :rtype: int
            """
            return self.rand40() % 10 + 1

        def rand49(self):
            """
            random integer in 0 ~ 48
            """
            return 7 * (rand7() - 1) + rand7() - 1

        def rand40(self):
            """
            random integer in 0 ~ 40
            """
            num = self.rand49()
            while num >= 40:
                num = self.rand49()
            return num
    ```