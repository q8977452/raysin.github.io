---
layout: post
title:  "30-Day LeetCoding Challenge Week2 "
date:   2020-04-23 23:10:30 +0800
categories: Leetcode
tags : Leetcode Python
mathjax: true
---
* Middle of the Linked List
    ```python
    def middleNode(self, head: ListNode) -> ListNode:
        slow, fast = head, head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
        return slow
    ```
	
    ```python
    def middleNode(self, head: ListNode) -> ListNode:
        if not head:
            return None
        
        curr = head
        count = 0
        index = {}
        while curr:
            index[count] = curr
            curr = curr.next
            count += 1
        
        mid = count//2
        
        return index[mid]
    ```
    
* Backspace String Compare
    - 字串
    ```python
    def backspaceCompare(self, S, T):
        """
        :type S: str
        :type T: str
        :rtype: bool
        """
        ans_S = ""
        ans_T = ""
        for s in S:
            if s == '#':
                if ans_S:
                    ans_S = ans_S[:-1]
            else:
                ans_S += s
        for t in T:
            if t == '#':
                if ans_T:
                    ans_T = ans_T[:-1]
            else:
                ans_T += t
        return ans_S == ans_T
    ```
    - pop
    ```python
    def backspaceCompare(self, S, T):
       """
       :type S: str
       :type T: str
       :rtype: bool
       """
       stackS, stackT = [], []
       for s in S:
           if s != "#":
               stackS.append(s)
           elif stackS:
               stackS.pop()
       for t in T:
           if t != "#":
               stackT.append(t)
           elif stackT:
               stackT.pop()
       return stackS == stackT
    ```
* Min Stack
    ```python
    class MinStack:

        def __init__(self):
            """
            initialize your data structure here.
            """
            self.stack = []
            self.min_states = []

        def push(self, x: int) -> None:
            self.stack.append(x)
            self.min_states.append(min(x, self.min_states[-1]) if self.min_states else x)

        def pop(self) -> None:
            self.stack.pop()
            self.min_states.pop()

        def top(self) -> int:
            return self.stack[-1]

        def getMin(self) -> int:
            return self.min_states[-1]  # 直接回傳當前 stack (某個 state) 的最小元素
    ```
* Diameter of Binary Tree
    ```python
    def diameterOfBinaryTree(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        self.ans = 0
        
        def max_depth(node):
            if not node:
                return 0
            left = max_depth(node.left)
            right = max_depth(node.right)
            self.ans = max(self.ans, left + right)
            return max(left, right) + 1
        
        max_depth(root)
        
        return self.ans
    ```
    
* Last Stone Weight
     * heapq
        * [python3中的heapq模块使用](https://zhuanlan.zhihu.com/p/65520009)
    * https://blog.csdn.net/fuxuemingzhu/article/details/91348483
    ```python
    def lastStoneWeight(self, stones):
        """
        :type stones: List[int]
        :rtype: int
        """
        stones = map(lambda x : -x, stones)
        heapq.heapify(stones)
        while len(stones) > 1:
            x = heapq.heappop(stones)
            if stones:
                y = heapq.heappop(stones)
                if x != y:
                    heapq.heappush(stones, -abs(x - y))
        return 0 if not stones else -stones[0]
    ```
    * https://blog.csdn.net/byr_wy/article/details/90437551
    ```python
    def lastStoneWeight(self, stones: List[int]) -> int:
        A = [-x for x in stones]
        heapq.heapify(A)
        while len(A)>1:
            x, y = -heapq.heappop(A), -heapq.heappop(A)
            heapq.heappush(A, -abs(x-y))
        return -A[0]
    ```
   
* Contiguous Array
    * https://blog.csdn.net/fuxuemingzhu/article/details/82667054
    ```python
    def findMaxLength(self, nums: List[int]) -> int:
        total_sum = 0
        index_map = {}
        index_map[0] = -1
        res = 0        
        for i, num in enumerate(nums):
            if num == 0:
                total_sum -= 1
            else:
                total_sum += 1
            if total_sum in index_map:
                res = max(res, i - index_map[total_sum])
            else:
                index_map[total_sum] = i
        return res
    ```
    
	* https://blog.csdn.net/ZJRN1027/java/article/details/80311009
    ```python
    def findMaxLength(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        dmap = {0 : -1}
        res = total = 0
        for i, n in enumerate(nums):
            total += 2 * nums[i] - 1
            if total in dmap:
                res = max(res, i - dmap[total])
            else:
                dmap[total] = i
        return res
    ```
* Perform String Shifts 
    * https://blog.csdn.net/erzhushashade/article/details/105516754
    ```python
    def stringShift(self, s: str, shift: List[List[int]]) -> str:
        
        if len(s)<2:
            return s
        
        length=len(s)
        sum=0
        
        for i in shift:
            sum=sum+(i[1] if i[0]==1 else -i[1])
        
        if sum==0:
            return s
        
        elif sum>0:
            sum=sum % length
            return s[length-sum:length]+s[:length-sum]
        
        else:
            sum=abs(sum) % length
            return s[sum:length]+s[0:sum]
    ```