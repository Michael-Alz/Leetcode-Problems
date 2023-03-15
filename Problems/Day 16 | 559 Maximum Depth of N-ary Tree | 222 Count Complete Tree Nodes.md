# Day 16 | 559 Maximum Depth of N-ary Tree | 222 Count Complete Tree Nodes

## 559 Maximum Depth of N-ary Tree

Problem:https://leetcode.com/problems/maximum-depth-of-n-ary-tree/description/

Solution: https://www.programmercarl.com/0104.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E5%A4%A7%E6%B7%B1%E5%BA%A6.html#%E8%BF%AD%E4%BB%A3%E6%B3%95

递归：

- Time complexity :  O(N)
- Space complexity : O(log(*N*))

~~~python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""

class Solution:
    def maxDepth(self, root: 'Node') -> int:
        #确定如果终止条件，如果到最下的节点就返回0
        if not root:
            return 0
        #给定初始值
        d = 0
        #遍历每个孩子节点，找到最深的节点
        for i in range(len(root.children)):
            d = max(self.maxDepth(root.children[i]),d)
        #到最深节点以后往回走每次加1
        return d + 1
~~~

层序遍历：

- Time complexity :  O(N)
- Space complexity : O(N)

~~~python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""

class Solution:
    def maxDepth(self, root: 'Node') -> int:
        if not root:
            return 0
        q = deque([root])
        #记录层深
        level = 0
        while q:
            n = len(q)
            for i in range(n):
                node = q.popleft()
                #把所有children加入queue
                for i in range(len(node.children)):
                    q.append(node.children[i])
            #每次遍历完一层就+1
            level += 1
        return level
~~~

## 222 Count Complete Tree Nodes

Problem: https://leetcode.com/problems/count-complete-tree-nodes/description/

Solution: https://www.programmercarl.com/0222.%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E8%8A%82%E7%82%B9%E4%B8%AA%E6%95%B0.html#python

层序遍历：

O(n)

O(n)

~~~python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
import collections
class Solution:
    def countNodes(self, root: TreeNode) -> int:
        queue = collections.deque()
        if root:
            queue.append(root)
        result = 0
        while queue:
            size = len(queue)
            for i in range(size):
                node = queue.popleft()
                result += 1 #记录节点数量
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
        return result
~~~

后序递归遍历:

- Time complexity : O(N)
- Space complexity : O(d)=O(log⁡N)

~~~python
import collections
class Solution:
    def countNodes(self, root: TreeNode) -> int:
        if not root:
            return 0
        #后序递归遍历
        left = self.countNodes(root.left)
        right = self.countNodes(root.right)
        value = left + right + 1
        return value
~~~

用完全二叉树性质递归：

- Time complexity : O(d^2)=O(log⁡^2N)where d is a tree depth.
- Space complexity : O(1)

```python
class Solution:
	    def countNodes(self, root: TreeNode) -> int:
				#确定终止条件
        if not root:
            return 0
				#如果当前二叉树不是满二叉树，就往下递归，总会有一个满二叉树
        left = root.left
        right = root.right
        leftDepth = 0 #这里初始为0是有目的的，为了下面求指数方便
        rightDepth = 0
        while left: #求左子树深度
            left = left.left
            leftDepth += 1
        while right: #求右子树深度
            right = right.right
            rightDepth += 1
				#判断当前节点下是否为满二叉树，是的话可以用公式求节点数量 2^d - 1
        if leftDepth == rightDepth:
            return (2 << leftDepth) - 1 #注意(2<<1) 相当于2^2，所以leftDepth初始为0
				#如果不是就往下寻找并且用平常二叉树的方法求节点，后序递归
        return self.countNodes(root.left) + self.countNodes(root.right) + 1
```