---
layout: post
title:  "October LeetCoding Challenge Week 2"
date:   2020-10-14 16:55:30 +0800
categories: Leetcode
tags : Leetcode, Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by October LeetCoding Challenge Week 2.





* Binary Search
    ```python
    def search(self, nums: List[int], target: int) -> int:
        l,r = 0 , len(nums)-1
        while l <= r:
            mid = (l + r) // 2
            if nums[mid] == target: return mid
            if nums[mid] < target:
                l = mid + 1
            else :
                r = mid - 1
        return -1
    ```
    
* Serialize and Deserialize BST
    ```python
    class Codec:

        def serialize(self, root: TreeNode) -> str:
            """Encodes a tree to a single string.
            """
            vals = []
            def preOrder(root):
                if root:
                    vals.append(root.val)
                    preOrder(root.left)
                    preOrder(root.right)
            preOrder(root)
            return ' '.join(map(str, vals))

        def deserialize(self, data: str) -> TreeNode:
            """Decodes your encoded data to tree.
            """
            vals = collections.deque(int(val) for val in data.split())
            def build(minVal, maxVal):
                if vals and minVal < vals[0] < maxVal:
                    val = vals.popleft()
                    root = TreeNode(val)
                    root.left = build(minVal, val)
                    root.right = build(val, maxVal)
                    return root
            return build(float('-inf'), float('inf'))
    ```
    
    
* Minimum Number of Arrows to Burst Balloons
    ```python
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        if not points: return 0
        points.sort(key = lambda x : x[1])
        curr_pos = points[0][1]
        ans = 1
        for i in range(len(points)):
            if curr_pos >= points[i][0]:
                continue
            curr_pos = points[i][1]
            ans += 1
        return ans
    ```
    
* Remove Duplicate Letters
    ```python
    def removeDuplicateLetters(self, s: str) -> str:
        count = collections.Counter(s)
        stack = []
        visited = collections.defaultdict(bool)
        for c in s:
            count[c] -= 1
            if visited[c]:
                continue
            while stack and count[stack[-1]] and stack[-1] > c:
                visited[stack[-1]] = False
                stack.pop()
            visited[c] = True
            stack.append(c)
        return "".join(stack)
    ```
    
* Buddy Strings
    ```python
    def buddyStrings(self, A: str, B: str) -> bool:
        if len(A) != len(B):
            return False
        diff = 0
        idxs = []
        for i, a in enumerate(A):
            if B[i] != a:
                diff += 1
                idxs.append(i)
        counter = dict()
        if diff == 0:
            for a in A:
                if a in counter and counter[a]:
                    return True
                else:
                    counter[a] = True
        if diff != 2:
            return False
        return A[idxs[0]] == B[idxs[1]] and A[idxs[1]] == B[idxs[0]]
    ```
    
* Sort List
    * Merge排序就是先劃分成一前一後等分的兩塊，然後對兩塊分別進行排序，然後再合併兩個有序序列。

        * 第一步
            * 如何等分地劃分?
                * 可以使用快慢指針的方式，當快指針到達結尾，那麼慢指針到了中間位置，把鍊錶進行截斷分成了兩個。

        * 第二步
            * 合併有序的序列，對於單鍊錶來說，正好用到了Merge Two Sorted Lists裡的把兩個鍊錶合併的方法。
    * Code
        ```python
        class Solution:
            def sortList(self, head: ListNode) -> ListNode:
                if head is None or head.next is None:
                    return head

                mid=self.getmiddle(head)
                lHead=head
                rHead=mid.next
                mid.next=None

                return self.merge(self.sortList(lHead),self.sortList(rHead))

            def getmiddle(self,head):

                if head is None:
                    return head

                fast=slow=head

                while fast.next and fast.next.next:
                    slow=slow.next
                    fast=fast.next.next

                return slow


            def merge(self,lHead,rHead):
                dumNode=ListNode(0)
                dumHead=dumNode

                i=lHead
                j=rHead

                while i and j:
                    if i.val<j.val:
                        dumNode.next=i
                        i=i.next
                    else:
                        dumNode.next=j
                        j=j.next
                    dumNode=dumNode.next

                if i:
                    dumNode.next=i
                if j:
                    dumNode.next=j

                return dumHead.next
        ```
    * Merge Two Sorted Lists
        ```python
        def mergeTwoLists(self, l1, l2):
            """
            :type l1: ListNode
            :type l2: ListNode
            :rtype: ListNode
            """
            head = ListNode(0)
            move = head
            if not l1: return l2
            if not l2: return l1
            while l1 and l2:
                if l1.val < l2.val:
                    move.next = l1
                    l1 = l1.next
                else:
                    move.next = l2
                    l2 = l2.next
                move = move.next
            move.next = l1 if l1 else l2
            return head.next
        ```
        
* House Robber II
    * 兩種偷的情況：
        * 第一種不偷最後一個房間
        * 第二種不偷第一個房間
    * 狀態轉移方程
        ```python
        dp[0] = num[0] （when i=0） 
        dp[1] = max(num[0], num[1]) （when i=1） 
        dp[i] = max(num[i] + dp[i - 2], dp[i - 1]) （when i !=0 and i != 1）
        ```
    * Code
        ```python
        class Solution:
            def rob(self, nums: List[int]) -> int:
                if not nums: return 0
                if len(nums) == 1: return nums[0]
                if len(nums) == 2: return max(nums[0], nums[1])
                N = len(nums)
                return max(self.rob_range(nums[0 : N - 1]), self.rob_range(nums[1 : N]))

            def rob_range(self, nums):
                if not nums: return 0
                if len(nums) == 1: return nums[0]
                if len(nums) == 2: return max(nums[0], nums[1])
                N = len(nums)
                dp = [0] * N
                dp[0] = nums[0]
                dp[1] = max(nums[0], nums[1])
                for i in range(2, N):
                    dp[i] = max(dp[i - 1], dp[i - 2] + nums[i])
                return dp[-1]
        ```