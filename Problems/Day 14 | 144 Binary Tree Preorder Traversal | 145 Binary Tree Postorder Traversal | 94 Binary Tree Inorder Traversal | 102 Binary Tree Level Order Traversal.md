# Day 14 | 144 Binary Tree Preorder Traversal | 145 Binary Tree Postorder Traversal | 94 Binary Tree Inorder Traversal | 102 Binary Tree Level Order Traversal

## 144 Binary Tree Preorder Traversal

Problem: https://leetcode.com/problems/binary-tree-preorder-traversal/

Solution: https://leetcode.com/problems/binary-tree-preorder-traversal/editorial/

### 递归

- Time complexity: O(n)
- Space complexity: O(n）

~~~python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        def pot(r):
            if r:
                res.append(r.val)
                pot(r.left)
                pot(r.right)
        
        pot(root)
        return res
~~~

### 迭代遍历

~~~python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        stack = [root]
        if root == None:
            return res
        while stack:
            #先处理中节点
            node = stack.pop()
            res.append(node.val)
            #右边入栈
            if node.right: #需要考虑当前节点是否有right节点
                stack.append(node.right)
            #左边入栈
            if node.left:
                stack.append(node.left)
        return res
~~~

### 统一迭代法

**就将访问的节点放入栈中，把要处理的节点也放入栈中但是要做标记。**

如何标记呢，**就是要处理的节点放入栈之后，紧接着放入一个空指针作为标记。** 这种方法也可以叫做标记法。

~~~python
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        st= []
        if root:
            st.append(root)
        while st:
            node = st.pop()
            if node != None:
                if node.right: #右
                    st.append(node.right)
                if node.left: #左
                    st.append(node.left)
                st.append(node) #中
                st.append(None)
            else:
                node = st.pop()
                result.append(node.val)
        return result
~~~

## 145 Binary Tree Postorder Traversal

Problem: https://leetcode.com/problems/binary-tree-postorder-traversal/description/

Solution: https://www.programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92%E9%81%8D%E5%8E%86.html

### 递归

~~~python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        def post_order(r):
            if r:
                post_order(r.left)
                post_order(r.right)
                res.append(r.val)
        post_order(root)
        return res
~~~

### 迭代遍历

~~~python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        #修改前序的代码中左右变成中右左
        #然后倒叙就是左右中就变成后序遍历了
        res = []
        stack = [root]
        if root == None:
            return res
        while stack:
            #先处理中节点
            node = stack.pop()
            res.append(node.val)
            #左边入栈
            if node.left: #需要考虑当前节点是否有left节点
                stack.append(node.left)
            #右边入栈
            if node.right:
                stack.append(node.right)
        return res[::-1]
~~~

### 统一迭代法

~~~python
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        st = []
        if root:
            st.append(root)
        while st:
            node = st.pop()
            if node != None:
                st.append(node) #中
                st.append(None)
                
                if node.right: #右
                    st.append(node.right)
                if node.left: #左
                    st.append(node.left)
            else:
                node = st.pop()
                result.append(node.val)
        return result
~~~

## 94 Binary Tree Inorder Traversal

Problem: https://leetcode.com/problems/binary-tree-inorder-traversal/

Solution: https://www.programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92%E9%81%8D%E5%8E%86.html

### 递归

~~~python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        def inorder(r):
            if r:
                inorder(r.left)
                res.append(r.val)
                inorder(r.right)
        inorder(root)
        return res
~~~

### 迭代遍历

~~~python
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        if not root:
            return []
        stack = []  # 不能提前将root结点加入stack中
        result = []
        cur = root
        while cur or stack:
            # 先迭代访问最底层的左子树结点
            if cur:     
                stack.append(cur)
                cur = cur.left		
            # 到达最左结点后处理栈顶结点    
            else:		
                cur = stack.pop()
                result.append(cur.val)
                # 取栈顶元素右结点
                cur = cur.right	
        return result
~~~

### 统一迭代法

~~~python
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        st = []
        if root:
            st.append(root)
        while st:
            node = st.pop()
            if node != None:
                if node.right: #添加右节点（空节点不入栈）
                    st.append(node.right)
                
                st.append(node) #添加中节点
                st.append(None) #中节点访问过，但是还没有处理，加入空节点做为标记。
                
                if node.left: #添加左节点（空节点不入栈）
                    st.append(node.left)
            else: #只有遇到空节点的时候，才将下一个节点放进结果集
                node = st.pop() #重新取出栈中元素
                result.append(node.val) #加入到结果集
        return result
~~~

## 102 Binary Tree Level Order Traversal

Problem:https://leetcode.com/problems/binary-tree-level-order-traversal/

Solution:https://www.programmercarl.com/0102.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86.html

### 层序遍历迭代法

~~~python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        res = []
        if not root:
            return res
        q = deque([root])
        while q:
            res0 = []
            size = len(q)
            #把一层变成一个list
            for i in range(size):
                node = q.popleft()
                res0.append(node.val)
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            #把这层的list放入结果list
            res.append(res0)
        return res
~~~

### 递归

~~~python
class Solution:
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        levels = []
        if not root:
            return levels
        
        def helper(node, level):
            # start the current level
            if len(levels) == level:
                levels.append([])

            # append the current node value
            levels[level].append(node.val)

            # process child nodes for the next level
            if node.left:
                helper(node.left, level + 1)
            if node.right:
                helper(node.right, level + 1)
            
        helper(root, 0)
        return levels
~~~

