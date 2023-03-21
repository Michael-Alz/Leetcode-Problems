# Day 21 | 530 Minimum Absolute Difference in BST | 501 Find Mode in Binary Search Tree | 236 Lowest Common Ancestor of a Binary Tree

## 530 Minimum Absolute Difference in BST

Problem: https://leetcode.com/problems/minimum-absolute-difference-in-bst/description/

Solution: https://programmercarl.com/0530.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E7%BB%9D%E5%AF%B9%E5%B7%AE.html#python

递归：

O(n)

O(n)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        bstlist = []
        m = float('inf')
        # 用中序递归把BST变成有序list
        def traverse_(root):
            if not root: 
                return 
            nonlocal bstlist
            traverse_(root.left)
            bstlist.append(root.val)
            traverse_(root.right)
            return bstlist
        traverse_(root)
        # 通过有序list寻找最小差
        for i in range(1, len(bstlist)):
            m = min(m, bstlist[i] - bstlist[i - 1])
        
        return 

#直接递归的时候找最小差值，用双指针
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        res = float('inf')
        pre = TreeNode(None)

        def traversal(root):
            nonlocal res, pre
            if root == None:
                return
            traversal(root.left)
            #第一次遍历到底的时候不会更新res
            if pre.val != None:
                res = min(res, root.val - pre.val)
            #回溯
            pre = root
            traversal(root.right)

        traversal(root)
        return res
```

迭代遍历：中序

```python
class Solution:
    def getMinimumDifference(self, root: TreeNode) -> int:
        stack = []
        cur = root
        pre = None
        result = float('inf')
        while cur or stack:
            if cur: # 指针来访问节点，访问到最底层
                stack.append(cur)
                cur = cur.left
            else: # 逐一处理节点
                cur = stack.pop()
                if pre: # 当前节点和前节点的值的差值
                    result = min(result, abs(cur.val - pre.val))
                pre = cur
                cur = cur.right
        return result
```

## 501 Find Mode in Binary Search Tree

Problem: https://leetcode.com/problems/find-mode-in-binary-search-tree/description/

Solution: https://programmercarl.com/0501.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E4%BC%97%E6%95%B0.html#java

先把BST中序遍历变成ordered list,再去找mode：

Time: O(n log n)

Space: O(n)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        bstlist = []
        def traversal(cur):
            nonlocal bstlist
            if not cur:
                return
            traversal(cur.left)
            bstlist.append(cur.val)
            traversal(cur.right)
        
        def mode(list):
            d = {}
            m = []
            for i in list:
                if i not in d:
                    d[i] = 1
                else:
                    d[i] += 1
            for key,val in d.items():
                if val == max(d.values()):
                    m.append(key)
            return m
        traversal(root)
        m = mode(bstlist)
        return m
```

直接中序遍历找众数：

O(n)

O(n)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        self.pre = TreeNode()
        self.maxcount = 0
        self.count = 0
        self.res = []

    def traversal(self,cur):
        if not cur:
            return
        self.traversal(cur.left) # 左
        # 中
        if self.pre == None: #遍历到叶节点
            self.count = 1
        elif self.pre.val == cur.val:
            self.count += 1
        else: self.count = 1
        self.pre = cur
        if self.count == self.maxcount:
            self.res.append(cur.val)
        # 更新result
        if self.count > self.maxcount:
            self.maxcount = self.count
            self.res.clear()
            self.res.append(cur.val)
        #右
        self.traversal(cur.right)
        return self.res

    def findMode(self, root: TreeNode) -> List[int]:
        if not root: return None
        return self.traversal(root)
```

## 236 Lowest Common Ancestor of a Binary Tree

Problem: https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/

Solution: https://programmercarl.com/0236.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html#java

后序递归：

Time: O(n)

Space: O(n)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        #确定终止条件，如果是空树return none，
        #如果找到p或则q节点，就可以直接往上返回当前节点,不用往下遍历
        if not root:
            return None
        if root == q or root == p:
            return root
        #左右中顺序遍历
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
				#如果左右都不为空，那么return当前节点
        if left != None and right != None:
            return root
				#左为空右不空，return右
        elif left == None and right != None:
            return right
        elif left != None and right == None:
            return left
```