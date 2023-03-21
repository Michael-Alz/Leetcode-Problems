# Day 22 | 235 Lowest Common Ancestor of a Binary Search Tree | 701 Insert into a Binary Search Tree | 450 Delete Node in a BST

## 235 Lowest Common Ancestor of a Binary Search Tree

Problem: https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/

Solution: https://programmercarl.com/0235.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html#%E9%80%92%E5%BD%92%E6%B3%95

可以用普通二叉树的方法，后序递归：

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

利用二叉树的特性递归：

如果p，q在当前节点值的中间，则中间的值为答案

如果当前节点大于p，q，则答案在节点左边

如果当前节点小于p，q，则答案在节点右边

```python
class Solution:
    """二叉搜索树的最近公共祖先 递归法"""

    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if root.val > p.val and root.val > q.val:
            return self.lowestCommonAncestor(root.left, p, q)
        if root.val < p.val and root.val < q.val:
            return self.lowestCommonAncestor(root.right, p, q)
        return root
```

迭代遍历：

```python
class Solution:
    """二叉搜索树的最近公共祖先 迭代法"""

    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        while True:
            if root.val > p.val and root.val > q.val:
                root = root.left
            elif root.val < p.val and root.val < q.val:
                root = root.right
            else:
                return root
```

## 701 Insert into a Binary Search Tree

Problem: https://leetcode.com/problems/insert-into-a-binary-search-tree/description/

Solution: https://programmercarl.com/0701.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E6%8F%92%E5%85%A5%E6%93%8D%E4%BD%9C.html#%E9%80%92%E5%BD%92

递归：

Time : O(n)

Space : O(n)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            root = TreeNode(val)
        if val < root.val:
            root.left = self.insertIntoBST(root.left, val)
        if val > root.val:
            root.right = self.insertIntoBST(root.right, val)
        return root
```

迭代遍历：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            return TreeNode(val)
        current = root
        while True:
            if current.val < val:
                if current.right:
                    current = current.right
                else:
                    current.right = TreeNode(val)
                    break
            elif current.val > val:
                if current.left:
                    current = current.left
                else:
                    current.left = TreeNode(val)
                    break
        return root
```

## 450 Delete Node in a BST

Problem:https://leetcode.com/problems/delete-node-in-a-bst/

Solution:https://programmercarl.com/0450.%E5%88%A0%E9%99%A4%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html

要注意五种情况：

- 第一种情况：没找到删除的节点，遍历到空节点直接返回了

- 找到删除的节点

  - 第二种情况：左右孩子都为空（叶子节点），直接删除节点， 返回NULL为根节点
  - 第三种情况：删除节点的左孩子为空，右孩子不为空，删除节点，右孩子补位，返回右孩子为根节点
  - 第四种情况：删除节点的右孩子为空，左孩子不为空，删除节点，左孩子补位，返回左孩子为根节点
  - 第五种情况：左右孩子节点都不为空，则将删除节点的左子树头结点（左孩子）放到删除节点的右子树的最左面节点的左孩子上，返回删除节点右孩子为新的根节点。

  Time complexity : O(log⁡N)

  Space complexity : O(H)

  ```python
  # Definition for a binary tree node.
  # class TreeNode:
  #     def __init__(self, val=0, left=None, right=None):
  #         self.val = val
  #         self.left = left
  #         self.right = right
  class Solution:
      def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
          # 当树是空树，返回None
          if not root:
              return None
          # 当在树里找到了key
          if root.val == key:
              # 第一种情况删除叶节点
              if root.left == None and root.right == None:
              #return None 因为可以用这个节点的父节点接住这个返回值，就达到删除的目的了
                  return None
              # 第二种情况
              elif root.left != None and root.right == None:
                  return root.left
              # 第三种情况
              elif root.left == None and root.right != None:
                  return root.right
              else:
                  # 当删除的节点左右都不为空，就要把左边的子树接入到右边子树的最左节点
                  cur = root.right
                  while cur.left != None:
                      cur = cur.left
                  # 把要删除的左子树连接到右边的左子树的最左叶子上
                  cur.left = root.left
                  # 变成了第三种情况
                  return root.right
          # 确定搜索方向
          if key < root.val:
              # 用左边接删除完节点的子树
              root.left = self.deleteNode(root.left,key)
          elif key > root.val:
              # 用右边接删除完节点的子树
              root.right = self.deleteNode(root.right,key)
          return root
  ```

  迭代法：

  ```python
  class Solution:
      def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
          # 找到节点后分两步，1. 把节点的左子树和右子树连起来，2. 把右子树跟父节点连起来
          # root is None
          if not root: return root
          p = root
          last = None
          while p:
              if p.val==key:
                  # 1. connect left to right
                  # right is not None -> left is None | left is not None
                  if p.right:
                      if p.left:
                          node = p.right
                          while node.left:
                              node = node.left
                          node.left = p.left
                      right = p.right
                  else:
                  # right is None -> right=left
                      right = p.left
                  # 2. connect right to last
                  if last==None:
                      root = right
                  elif last.val>key:
                      last.left = right
                  else:
                      last.right = right
                  # 3. return
                  break
              else:
                  # Update last and continue
                  last = p
                  if p.val>key:
                      p = p.left
                  else:
                      p = p.right
          return root
  ```