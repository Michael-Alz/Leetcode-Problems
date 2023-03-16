# Day 17 ｜ 110 Balanced Binary Tree | 257 Binary Tree Paths | 404 Sum of Left Leaves

## 110 Balanced Binary Tree

Problem:https://leetcode.com/problems/balanced-binary-tree/

Solution: https://www.programmercarl.com/0110.%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E9%A2%98%E5%A4%96%E8%AF%9D

递归后序：左右中的顺序

- Time complexity : O(n)

  For every subtree, we compute its height in constant time as well as compare the height of its children.

- Space complexity : O(n). The recursion stack may go up to O(n) if the tree is unbalanced.

```python
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        h = self.max_height(root)
        if h == -1:
            return False
        else: 
            return True
     #求高度就是左右中顺序，因为需要左右节点的高度来判断是否平衡
    def max_height(self,root):
        #递归终止条件如果到底了就返回0
        if not root:
            return 0
        #如果左边子树不是平衡二叉树，直接可以返回-1
        left_balance = self.max_height(root.left)
        if left_balance == -1:
            return -1
        #如果右边子树不是平衡二叉树，直接可以返回-1
        right_balance = self.max_height(root.right) 
        if right_balance ==  -1:
            return -1
        #如果下面子树都是平衡二叉树，就计算当前节点子树的高度差，
        #如果大于1，就不为平衡二叉树，返回-1
        if abs(right_balance - left_balance) > 1:
            return -1
        #如果是平衡的，就取高度最大的子树的高度再加上当前的节点（就是+1）
        else:
            return max(right_balance,left_balance) + 1
```

迭代法：

```python
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
				#用字典记录子树的高度
        height_map = {}
        stack = [root]
        while stack:
            node = stack.pop()
            if node:
								#如果后面还有节点，就放入空节点
                stack.append(node)
                stack.append(None)
                if node.left: stack.append(node.left)
                if node.right: stack.append(node.right)
            else:
                real_node = stack.pop()
								#获取当前节点的子树高度
                left, right = height_map.get(real_node.left, 0), height_map.get(real_node.right, 0)
                if abs(left - right) > 1:
                    return False
								#把当前节点的高度放入字典更新real_node的高度
                height_map[real_node] = 1 + max(left, right)
        return True
```

## 257 Binary Tree Paths

Problem: https://leetcode.com/problems/binary-tree-paths/

Solution: https://www.programmercarl.com/0257.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%89%80%E6%9C%89%E8%B7%AF%E5%BE%84.html#python

递归（隐形回溯）:

https://www.notion.so/Binary-Tree-Paths-2a8d77049a6e4c45ba480b67d417622f?pvs=4#88e0a3a947014bb7b748ca83285faebb

递归回溯的原理正是因为每次函数调用时，都会在内存中创建一个新的栈帧来保存本地变量、参数以及返回地址。当函数返回时，当前栈帧会被销毁，本地变量会被丢弃。这个过程使得算法能够回到之前的状态，从而在上一层尝试其他可能性。

- Time complexity : O(N)
- Space complexity : O(logN)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        result = []
        path = ''
        if not root:
            return result
        else: 
            self.get_path(root,path,result)
            return result 
        return self.get_path(root)
    def get_path(self,root,path,result):
        path += str(root.val)
        # 若当前节点为leave，直接输出
        if root.left == None and root.right == None:
            result.append(path)
        # + '->' 是隐藏回溯
        if root.left:
            self.get_path(root.left, path + '->', result)
        if root.right:
            self.get_path(root.right, path + '->', result)
				#叶节点走完最后一层，会回到之前的循环中，变量会回溯到之前的值
```

深度优先遍历：

- Time complexity : O(N)
- Space complexity : O(N)

```python
from collections import deque

class Solution:
    """二叉树的所有路径 迭代法"""

    def binaryTreePaths(self, root: TreeNode) -> List[str]:
        # 题目中节点数至少为1
        stack, path_st, result = deque([root]), deque(), []
        path_st.append(str(root.val))

        while stack:
            cur = stack.pop()
            path = path_st.pop()
            # 如果当前节点为叶子节点，添加路径到结果中
            if not (cur.left or cur.right):
                result.append(path)
            if cur.right:
                stack.append(cur.right)
                path_st.append(path + '->' + str(cur.right.val))
            if cur.left:
                stack.append(cur.left)
                path_st.append(path + '->' + str(cur.left.val))

        return result
```

## 404 Sum of Left Leaves

Problem:https://leetcode.com/problems/sum-of-left-leaves/description/

Solution: https://www.programmercarl.com/0404.%E5%B7%A6%E5%8F%B6%E5%AD%90%E4%B9%8B%E5%92%8C.html#%E8%A7%86%E9%A2%91%E8%AE%B2%E8%A7%A3

层序遍历：

- Time complexity : O(N)
- Space complexity : O(N)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        elif not root.left and not root.right:
            return 0
        q = deque([root])
        res = 0
        while q:
            node = q.popleft()
            #判断当前节点的左节点如果为叶节点则符合条件
            if node.left and (not node.left.left and not node.left.right):
                res += node.left.val
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)    
        return res
```

递归：后序遍历

- Time complexity : O(N)
- Space complexity : O(N)

```python
class Solution:
    def sumOfLeftLeaves(self, root: TreeNode) -> int:
        if not root: 
            return 0
        
        left_left_leaves_sum = self.sumOfLeftLeaves(root.left)  # 左
        right_left_leaves_sum = self.sumOfLeftLeaves(root.right) # 右
        
        cur_left_leaf_val = 0
        if root.left and not root.left.left and not root.left.right: 
            cur_left_leaf_val = root.left.val 
            
        return cur_left_leaf_val + left_left_leaves_sum + right_left_leaves_sum # 中
```