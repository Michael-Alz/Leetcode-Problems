# Day 23 | 669 Trim a Binary Search Tree | 108 Convert Sorted Array to Binary Search Tree | 538 Convert BST to Greater Tree

## 669 Trim a Binary Search Tree

Problem: https://leetcode.com/problems/trim-a-binary-search-tree/description/

Solution: https://programmercarl.com/0669.%E4%BF%AE%E5%89%AA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#%E9%80%92%E5%BD%92%E6%B3%95

递归：

- Time Complexity: O(N), where N is the total number of nodes in the given tree. We visit each node at most once.
- Space Complexity: O(N)). Even though we don't explicitly use any additional memory, the call stack of our recursion could be as large as the number of nodes in the worst case.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def trimBST(self, root: Optional[TreeNode], low: int, high: int) -> Optional[TreeNode]:
        if not root:
            return None
        # 如果当前节点比low小，那么这个节点的左子树一定都比low小，
        # 但右子树有可能有在区间里的节点，下面同理
        if root.val < low:
            return self.trimBST(root.right, low, high)
        if root.val > high:
            return self.trimBST(root.left, low, high)
        # 用左右子树去接修剪完的子树
        root.left = self.trimBST(root.left, low, high)
        root.right = self.trimBST(root.right, low, high)
        return root
```

## 108 Convert Sorted Array to Binary Search Tree

Problem:https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/

Solution: https://programmercarl.com/0108.%E5%B0%86%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E8%BD%AC%E6%8D%A2%E4%B8%BA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#python

递归：

把数组分为左右两边，再分别进行递归

Time: O(n)

Space: O(logN)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        if not nums:
            return None
        # 中间节点为root
        mid = (len(nums) - 1) // 2
        left = nums[ : mid]
        right = nums[mid + 1 :]
        root = TreeNode(nums[mid])
        # 分别递归遍历左和右区间，分别为左子树和右子树
        root.left = self.sortedArrayToBST(left)
        root.right = self.sortedArrayToBST(right)
        return root
```

## 538 Convert BST to Greater Tree

Problem: https://leetcode.com/problems/convert-bst-to-greater-tree/description/

Solution: https://programmercarl.com/0538.%E6%8A%8A%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E8%BD%AC%E6%8D%A2%E4%B8%BA%E7%B4%AF%E5%8A%A0%E6%A0%91.html

递归：右中左的顺序遍历

- Time complexity : O(n)
- Space complexity : O(n)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def convertBST(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        # 用一个pre指针来保存右边的值
        pre = TreeNode(0)
        def Traversal(root):
            if not root:
                return None
            Traversal(root.right)
            # 把右边的值加到节点
            pre.val += root.val
            root.val = pre.val
            Traversal(root.left)
            return root
        return Traversal(root)
```

迭代：

```python
class Solution(object):
    def convertBST(self, root):
        total = 0
        
        node = root
        stack = []
        while stack or node is not None:
            # push all nodes up to (and including) this subtree's maximum on
            # the stack.
            while node is not None:
                stack.append(node)
                node = node.right

            node = stack.pop()
            total += node.val
            node.val = total

            # all nodes with values between the current and its parent lie in
            # the left subtree.
            node = node.left

        return root
```

## 二叉树总结

<img src="https://code-thinking-1253855093.file.myqcloud.com/pics/20211030125421.png" alt="img" style="zoom:200%;" />



https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E6%80%BB%E7%BB%93%E7%AF%87.html#%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80