---
layout: post
title:  "November LeetCoding Challenge Week 1"
date:   2020-11-07 16:24:30 +0800
categories: Leetcode
tags : Leetcode Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by November LeetCoding Challenge Week 1.




* Convert Binary Number in a Linked List to Integer
    * 分析
        * Convert n to list-node
        * Calculate binary to decimal based on location
    * 程式
        ```python
        def getDecimalValue(self, head: ListNode) -> int:
            t, node_n=head, 0
            while(t):
                node_n+=1
                t=t.next
            sum_n=0
            for i in range(node_n):
                x=head.val
                sum_n+=x*2**(node_n-1-i)
                head=head.next
            return sum_n
        ```
        
* Insertion Sort List
    * Algorithm of Insertion Sort:
        1. Insertion sort iterates, consuming one input element each repetition, and growing a sorted output list.
        2. At each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there.
        3. It repeats until no input elements remain.
    
    * 程式
        ```python
        def insertionSortList(self, head: ListNode) -> ListNode:
            if not head or not head.next: return head
            root = TreeNode(0)
            root.next = head
            while head.next:
                if head.val <= head.next.val:
                    head = head.next
                else:
                    temp = head.next
                    q = root
                    head.next = head.next.next
                    while q.next and q.next.val < temp.val:
                        q = q.next
                    temp.next = q.next
                    q.next = temp
            return root.next
        ```
* Consecutive Characters
    ```python
    def maxPower(self, s: str) -> int:
        if len(s) == 1:
            return 1
        tmp = [x for x in s]
        a, b = 1, 1
        for i in range(1, len(tmp)):
            if tmp[i] != tmp[i-1]:
                a = 1
            else:
                a += 1
                b = max(b,a)
                
        return b
    ```
    
* Minimum Height Trees
    * BFS
        ```python
        def findMinHeightTrees(self, n: int, edges: List[List[int]]) -> List[int]:
            if n == 1: return [0]
            leaves = collections.defaultdict(set)
            for u, v in edges:
                leaves[u].add(v)
                leaves[v].add(u)
            que = collections.deque()
            for u, vs in leaves.items():
                if len(vs) == 1:
                    que.append(u)
            while n > 2:
                _len = len(que)
                n -= _len
                for _ in range(_len):
                    u = que.popleft()
                    for v in leaves[u]:
                        leaves[v].remove(u)
                        if len(leaves[v]) == 1:
                            que.append(v)
            return list(que)
        ```
        
* Minimum Cost to Move Chips to The Same Position
    * 分析
    
	給你一個數組，如果同為偶數或者奇數，則代價是0，否則代價為1
    
	* 程式
        ```python
        def minCostToMoveChips(self, position: List[int]) -> int:
            counter=collections.Counter(position)

            cnt=0
            for pos in position:
                if(pos%2==0):
                    cnt+=1
            return min(cnt,len(position)-cnt)
        ```
        
* Find the Smallest Divisor Given a Threshold
    * Bisection
        ```python
        def smallestDivisor(self, nums: List[int], threshold: int) -> int:
            import math
            lo,hi=1,max(nums)
            res = 1000000
            while lo<=hi:
                mid=(lo+hi)//2
                t = sum(math.ceil(a/mid) for a in nums)
                if t<=threshold:
                    res=min(res,mid)
                    hi = mid - 1
                else:
                    lo = mid + 1

            return res
        ```
        
* Add Two Numbers II
    * 先求和再構成列表
        ```python
        def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
            num1 = ''
            num2 = ''
            while l1:
                num1 += str(l1.val)
                l1 = l1.next
            while l2:
                num2 += str(l2.val)
                l2 = l2.next
            add = str(int(num1) + int(num2))
            head = ListNode(add[0])
            answer = head
            for i in range(1, len(add)):
                node = ListNode(add[i])
                head.next = node
                head = head.next
            return answer
        ```