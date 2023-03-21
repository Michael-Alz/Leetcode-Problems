# Day 20 | 654 Maximum Binary Tree | 617 Merge Two Binary Trees | 700 Search in a Binary Search Tree | 98 Validate Binary Search Tree

## 654 Maximum Binary Tree

Problem: https://leetcode.com/problems/maximum-binary-tree/description/

Solution: https://programmercarl.com/0654.%E6%9C%80%E5%A4%A7%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

递归：

找到最大值然后分别递归最大值左右两边

Time: O(n)

Space:  O(n)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> Optional[TreeNode]:
        if not nums:
            return None
        # 找到数组中的最大值的index   
        maxval = max(nums)
        maxi = nums.index(maxval)
        node = TreeNode()
        node.val = maxval
        #最大值的左边
        left = nums[ : maxi] #左闭右开
        #最大值右边
        right = nums[maxi + 1 : ] 
        node.left = self.constructMaximumBinaryTree(left)
        node.right = self.constructMaximumBinaryTree(right)
        return node
```

## 617 Merge Two Binary Trees

Problem: https://leetcode.com/problems/merge-two-binary-trees/editorial/

Solution: https://programmercarl.com/0617.%E5%90%88%E5%B9%B6%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E6%80%9D%E8%B7%AF

递归:

更改一棵树

Time: O(n)

Space: O(n)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        # 如果遍历到root1没有节点，return root2
        if not root1:
            return root2
        # 如果遍历到root2没有节点，return root1
        if not root2:
            return root1
        root1.val += root2.val
				# 如果左边有值，右边没有，后面递归方程那返回的结果就是左边的
        root1.left = self.mergeTrees(root1.left, root2.left)
        root1.right = self.mergeTrees(root1.right, root2.right)
        return root1
```

## 700 Search in a Binary Search Tree

Problem: https://leetcode.com/problems/search-in-a-binary-search-tree/description/

Solution: https://leetcode.com/problems/search-in-a-binary-search-tree/description/

遍历：

- Time complexity : O(H), where H is a tree height. That results in O(log⁡N in the average case, and O(N) in the worst case.
- Space complexity : O(1)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        # 当root被迭代完了就返回None
        while root != None: 
            if root.val == val:
                return root
            #如果值比当前节点小，那一定在左边
            elif root.val > val:
                root = root.left
            #如果值比当前节点大，那一定在右边
            elif root.val < val:
                root = root.right
        return None
```

递归：

- Time complexity : O(H), where H is a tree height. That results in O(log⁡N in the average case, and O(N) in the worst case.
- Space complexity : O(H) to keep the recursion stack, i.e. O(log⁡N) in the average case, and O(N) in the worst case.

```python
class Solution:
    def searchBST(self, root: TreeNode, val: int) -> TreeNode:
        # 为什么要有返回值:
        #   因为搜索到目标节点就要立即return，
        #   这样才是找到节点就返回（搜索某一条边），如果不加return，就是遍历整棵树了。

        if not root or root.val == val:
            return root

        if root.val > val:
            return self.searchBST(root.left, val)

        if root.val < val:
            return self.searchBST(root.right, val)
```

## 98 Validate Binary Search Tree

Problem: https://leetcode.com/problems/validate-binary-search-tree/

Solution: https://programmercarl.com/0098.%E9%AA%8C%E8%AF%81%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#%E8%BF%AD%E4%BB%A3%E6%B3%95

利用搜索二叉树的性质：

- Time complexity : O(N)
- Space complexity : O(N)

```python
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        # 思路: 利用BST中序遍历的特性.
        # 中序遍历输出的二叉搜索树节点的数值是有序序列
        candidate_list = []
        # 用中序遍历把节点的值放入list
        def __traverse(root: TreeNode) -> None: 
            nonlocal candidate_list
            if not root: 
                return 
            __traverse(root.left)
            candidate_list.append(root.val)
            __traverse(root.right)
        # 判断是否为有序list
        def __is_sorted(nums: list) -> bool: 
            for i in range(1, len(nums)): 
                if nums[i] <= nums[i - 1]: # 注意: Leetcode定义二叉搜索树中不能有重复元素
                    return False
            return True
        # 如果不是的话说明不是二叉搜索树
        __traverse(root)
        res = __is_sorted(candidate_list)
        
        return res
```

递归：

```python
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        # 规律: BST的中序遍历节点数值是从小到大. 
        cur_max = -float("INF")
        def __isValidBST(root: TreeNode) -> bool: 
            nonlocal cur_max
            
            if not root: 
                return True
            
            is_left_valid = __isValidBST(root.left)
            if cur_max < root.val: 
                cur_max = root.val
            else: 
                return False
            is_right_valid = __isValidBST(root.right)
            
            return is_left_valid and is_right_valid
        return __isValidBST(root)
```

迭代（中序遍历）：

```python
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        # method 2
        que, pre = [], None
        while root or que:
            while root:
                que.append(root)
                root = root.left
            root = que.pop()
            # 对第一个节点只做记录，对后面的节点进行比较
            if pre is None:
                pre = root.val
            else:
                if pre >= root.val: return False
                pre = root.val
            root = root.right
        return True
```