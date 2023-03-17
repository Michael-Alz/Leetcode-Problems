# Day 18 | 513 Find Bottom Left Tree Value | 112,113 Path Sum | 105,106 Construct Binary Tree from Preorder (Postorder) and Inorder Traversal

 ## 513 Find Bottom Left Tree Value

Problem: https://leetcode.com/problems/find-bottom-left-tree-value/description/

Solution: https://programmercarl.com/0513.%E6%89%BE%E6%A0%91%E5%B7%A6%E4%B8%8B%E8%A7%92%E7%9A%84%E5%80%BC.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

迭代 - 层序遍历:

Time: O(n)

Space: O(height)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        q = deque([root])
        res = 0
        while q:
            size = len(q)
            for i in range(size):
                #这里需要先把res替换为当前最靠左的答案：
                #如果树就一个节点，就返回这个节点的值；
                #如果这个节点下面只有右边节点，就返回右边节点的值；
                #否则返回左边节点的值
                if i == 0:
                    res = q[0].val
                node = q.popleft()
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
        return res
```

递归：

```python
class Solution:
    def findBottomLeftValue(self, root: TreeNode) -> int:
        max_depth = -float("INF")
        leftmost_val = 0

        def __traverse(root, cur_depth): 
						#nonloacl 可以修改上层变量的值
            nonlocal max_depth, leftmost_val
						#判断是否为叶子节点
            if not root.left and not root.right: 
                if cur_depth > max_depth: 
                    max_depth = cur_depth
                    leftmost_val = root.val  
            if root.left: #回溯,可以写成 __traverse(root.left, cur_depth + 1)
                cur_depth += 1
                __traverse(root.left, cur_depth)
                cur_depth -= 1
            if root.right: #回溯,可以写成 __traverse(root.right, cur_depth + 1)
                cur_depth += 1
                __traverse(root.right, cur_depth)
                cur_depth -= 1

        __traverse(root, 0)
        return leftmost_val
```

## 112 Path Sum

Problem: https://leetcode.com/problems/path-sum/description/

Solution: https://leetcode.com/problems/path-sum/editorial/

遍历：DFS

- Time complexity : O(*N*).
- Space complexity : O(*N*) since in the worst case, when the tree is completely unbalanced, *e.g.* each node has only one child node, we would keep all N nodes in the stack. But in the best case (the tree is balanced), the height of the tree would be log⁡(N). Therefore, the space complexity in this case would be O(log⁡(N)).

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        #DFS
        if not root:
            return False
        #需要用两个栈来保存节点和节点路径相加的值
        stack = [root]
        sum_ = [root.val]
        while stack:
            node = stack.pop()
            cur_sum = sum_.pop()
            #当到叶节点以后，与target判断
            if (not node.left and not node.right) and cur_sum == targetSum:
                return True   
            if node.left:
                stack.append(node.left)
                sum_.append(cur_sum + node.left.val)
            if node.right:
                stack.append(node.right)
                sum_.append(cur_sum + node.right.val)
        return False
```

递归：

```python
class Solution:
    def hasPathSum(self, root, sum):
        """
        :type root: TreeNode
        :type sum: int
        :rtype: bool
        """
        if not root:
            return False

        sum -= root.val
        if not root.left and not root.right:  # if reach a leaf
            return sum == 0
        return self.hasPathSum(root.left, sum) or self.hasPathSum(root.right, sum)
```

## 113 Path Sum II

Problem: https://leetcode.com/problems/path-sum-ii/description/

Solution: https://programmercarl.com/0112.%E8%B7%AF%E5%BE%84%E6%80%BB%E5%92%8C.html

DFS：

T: O(n)

S: O(n)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        #DFS
        res = []
        if not root:
            return res
        #需要用两个栈来保存节点和节点路径相加的值
        stack, temp = [root], [(root.val, [root.val])]
        while stack:
            node = stack.pop()
            value, path = temp.pop()
            #当到叶节点以后，与target判断
            if (not node.left and not node.right) and value == targetSum:
                res.append(path) 
            if node.left:
                stack.append(node.left)
                #两个list相加会把list里的元素放到一起
                temp.append((node.left.val+value, path+[node.left.val]))
            if node.right:
                stack.append(node.right)
                temp.append((node.right.val+value, path+[node.right.val]))
        return res
```

BFS(层序遍历)：

```python
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        if not root:
            return []
				#用两个个队列保存目前的总和与路径
        que, temp = deque([root]), deque([(root.val, [root.val])])
        result = []
        while que:
            for _ in range(len(que)):
                node = que.popleft()
                value, path = temp.popleft()
                if (not node.left) and (not node.right):
                    if value == targetSum:
                        result.append(path)
                if node.left:
                    que.append(node.left)
                    temp.append((node.left.val+value, path+[node.left.val]))
                if node.right:
                    que.append(node.right)
                    temp.append((node.right.val+value, path+[node.right.val]))
        return result
```

递归：

```python
class solution:
    def pathsum(self, root: treenode, targetsum: int) -> list[list[int]]:

        def traversal(cur_node, remain):
            if not cur_node.left and not cur_node.right:
                if remain == 0:
                    result.append(path[:])
                return

            if cur_node.left:
                path.append(cur_node.left.val)
                traversal(cur_node.left, remain-cur_node.left.val)
                path.pop()

            if cur_node.right:
                path.append(cur_node.right.val)
                traversal(cur_node.right, remain-cur_node.right.val)
                path.pop()

        result, path = [], []
        if not root:
            return []
        path.append(root.val)
        traversal(root, targetsum - root.val)
        return result
```

## 106 Construct Binary Tree from Inorder and Postorder Traversal

Problem: https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/

Solution: https://programmercarl.com/0106.%E4%BB%8E%E4%B8%AD%E5%BA%8F%E4%B8%8E%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86%E5%BA%8F%E5%88%97%E6%9E%84%E9%80%A0%E4%BA%8C%E5%8F%89%E6%A0%91.html

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/470a223f-d666-4bf1-84eb-ed53876e174f/Untitled.png)

递归：

- 第一步：如果数组大小为零的话，说明是空节点了。
- 第二步：如果不为空，那么取后序数组最后一个元素作为节点元素。
- 第三步：找到后序数组最后一个元素在中序数组的位置，作为切割点
- 第四步：切割中序数组，切成中序左数组和中序右数组 （顺序别搞反了，一定是先切中序数组）
- 第五步：切割后序数组，切成后序左数组和后序右数组
- 第六步：递归处理左区间和右区间

Time complexity : O(N)

Space complexity : O(N)

```python
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> Optional[TreeNode]:
        # 第一步: 特殊情况讨论: 树为空. 或者说是递归终止条件
        if not postorder:
            return

        # 第二步: 后序遍历的最后一个就是当前的中间节点
        root_val = postorder[-1]
        root = TreeNode(root_val)

        # 第三步: 找切割点.
        root_index = inorder.index(root_val)

        # 第四步: 切割inorder数组. 得到inorder数组的左,右半边.
        left_inorder = inorder[:root_index]
        right_inorder = inorder[root_index + 1:]

        # 第五步: 切割postorder数组. 得到postorder数组的左,右半边.
        # ⭐️ 重点1: 中序数组大小一定跟后序数组大小是相同的.
        left_postorder = postorder[:len(left_inorder)]
        right_postorder = postorder[len(left_inorder): len(postorder) - 1]

        # 第六步: 递归
        root.left = self.buildTree(left_inorder, left_postorder)
        root.right = self.buildTree(right_inorder, right_postorder)

        # 第七步: 返回答案
        return root
```

## 105 Construct Binary Tree from Preorder and Inorder Traversal

Problem: https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/

Solution: https://programmercarl.com/0106.%E4%BB%8E%E4%B8%AD%E5%BA%8F%E4%B8%8E%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86%E5%BA%8F%E5%88%97%E6%9E%84%E9%80%A0%E4%BA%8C%E5%8F%89%E6%A0%91.html#python-2

递归：

- 第一步：如果数组大小为零的话，说明是空节点了。
- 第二步：如果不为空，那么取后序数组第一个元素作为节点元素。
- 第三步：找到前序数组第一个元素在中序数组的位置，作为切割点
- 第四步：切割中序数组，切成中序左数组和中序右数组 （顺序别搞反了，一定是先切中序数组）
- 第五步：切割中序数组，切成中序左数组和中序右数组
- 第六步：递归处理左区间和右区间
- Time complexity : O(N)
- Space complexity : O(N)

```python
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        # 第一步: 特殊情况讨论: 树为空. 或者说是递归终止条件
        if not preorder:
            return None

        # 第二步: 前序遍历的第一个就是当前的中间节点.
        root_val = preorder[0]
        root = TreeNode(root_val)

        # 第三步: 找切割点.
        separator_idx = inorder.index(root_val)

        # 第四步: 切割inorder数组. 得到inorder数组的左,右半边.
        inorder_left = inorder[:separator_idx]
        inorder_right = inorder[separator_idx + 1:]

        # 第五步: 切割preorder数组. 得到preorder数组的左,右半边.
        # ⭐️ 重点1: 中序数组大小一定跟前序数组大小是相同的.
        preorder_left = preorder[1:1 + len(inorder_left)]
        preorder_right = preorder[1 + len(inorder_left):]

        # 第六步: 递归
        root.left = self.buildTree(preorder_left, inorder_left)
        root.right = self.buildTree(preorder_right, inorder_right)

        return root
```