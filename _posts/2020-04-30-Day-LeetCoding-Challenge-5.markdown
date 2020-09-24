---
layout: post
title:  "30-Day LeetCoding Challenge Week5"
date:   2020-04-30 23:40:30 +0800
categories: Leetcode
tags : Leetcode Python 
mathjax: true
---
* content 
{:toc}
Practice Coding by 30-Day LeetCoding Challenge Week5.



*   Binary Tree Maximum Path Sum

    *   https://blog.csdn.net/qqxx6661/article/details/78484940
        * 利用self
		
        ```python
        def maxPathSum(self, root):
            """
            :type root: TreeNode
            :rtype: int
            """
            self.maxSum = float('-inf')
            self._maxPathSum(root)
            return self.maxSum
        def _maxPathSum(self, root):  # DFS
            if root is None:
                return 0
            left = self._maxPathSum(root.left)
            right = self._maxPathSum(root.right)
            left = left if left > 0 else 0
            right = right if right > 0 else 0
            self.maxSum = max(self.maxSum, root.val + left + right)

            return max(left, right) + root.val
        ```
		
        * 雙重函數
        
		```python
        def maxPathSum(self, root: TreeNode) -> int:
            def _maxPathSum(root):
                nonlocal total

                if root is None:
                    return 0

                left = max(_maxPathSum(root.left), 0)
                right = max(_maxPathSum(root.right), 0)
                total = max(total, root.val + left + right)

                return max(left, right) + root.val
        
            total = float("-inf") 
            _maxPathSum(root)

            return total
        ```
        
* Check If a String Is a Valid Sequence from Root to Leaves Path in a Binary Tree
    * https://www.youtube.com/watch?v=_Es-FEkjKmA
    * [geeksforgeeks](https://www.geeksforgeeks.org/check-root-leaf-path-given-sequence/)
        ```python
        def isValidSequence(self, root: TreeNode, arr: List[int]) -> bool:
            l = len(arr)
            i = 0
            return self.helper(root, arr, l, i)

        def helper(self, root, arr, l, i):
            if root == None:
                return l == 0
            if (i == l - 1) and (root.left == None and root.right == None) and (root.val == arr[i]):
                return True

            if (i < l) and (root.val == arr[i]):
                return (self.helper(root.left, arr, l ,i+1) or self.helper(root.right, arr, l ,i+1))
        ```
    * [c++](https://www.cnblogs.com/qwfand/p/12810961.html)
        ```cpp
        public:
            bool isValidSequence(TreeNode* root, vector<int>& arr) {
                return helper(root,arr,0);
            }
            bool helper(TreeNode * node,vector<int>& arr, int pos)
            {
                if(node==NULL)
                    return false;
                if(node->val!=arr[pos])
                   return false;

                if(pos==arr.size()-1)//數組完了，判斷當前節點是不是葉節點數組完了
                    return (node->left==NULL && node->right==NULL);

                return(helper(node->left,arr,pos+1)||helper(node->right,arr,pos+1));
            }
        ```