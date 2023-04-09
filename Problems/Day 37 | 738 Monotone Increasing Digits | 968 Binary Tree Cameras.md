# Day 37 | 738 Monotone Increasing Digits | 968 Binary Tree Cameras

## 738 Monotone Increasing Digits

Problem: https://leetcode.com/problems/monotone-increasing-digits/

Solution: https://programmercarl.com/0738.%E5%8D%95%E8%B0%83%E9%80%92%E5%A2%9E%E7%9A%84%E6%95%B0%E5%AD%97.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

- 时间复杂度：O(n)，n 为数字长度
- 空间复杂度：O(n)，需要一个字符串，转化为字符串操作更方便

```python
class Solution:
    def monotoneIncreasingDigits(self,n: int) -> int:
        nums = list(str(n))
        # 从后向前遍历
        for i in range(len(nums) - 2, -1, -1):
            # 如果后一位比前一位大的话，就把后面的都变成9然后把当前位 --1
            if int(nums[i + 1]) < int(nums[i]):
                nums[i] = str(int(nums[i]) - 1)
                nums[i + 1 :] = len(nums[i + 1 :]) * '9'
        return int(''.join(nums))
```

## 968 Binary Tree Cameras

Problem:https://leetcode.com/problems/binary-tree-cameras/description/

Solution: https://programmercarl.com/0968.%E7%9B%91%E6%8E%A7%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E6%80%9D%E8%B7%AF

- Time Complexity: O(N), where N is the number of nodes in the given tree.
- Space Complexity: O(H), where H is the height of the given tree.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
												
												# 0：该节点无覆盖
												# 1：本节点有摄像头
												# 2：本节点有覆盖
												# Greedy Algo:
												# 从下往上安装摄像头：跳过leaves这样安装数量最少，局部最优 -> 全局最优
												# 先给leaves的父节点安装，然后每隔两层节点安装一个摄像头，直到Head

class Solution:
    def __init__(self):
        self.res = 0
    def traversal(self,cur):
        # 为了让子节点的父节点放摄像头，遍历到空节点要返回有覆盖
        if not cur :
            return 2
        # 左右中顺序遍历
        left = self.traversal(cur.left)
        right = self.traversal(cur.right)
        # 情况一，左右都有覆盖,父节点为无覆盖
        if left == 2 and right == 2:
            return 0
        # 情况二，左右有一边没有覆盖，父节点需要放入摄像头
								# left == 0 && right == 0 左右节点无覆盖
                # left == 1 && right == 0 左节点有摄像头，右节点无覆盖
                # left == 0 && right == 1 左节点有无覆盖，右节点摄像头
                # left == 0 && right == 2 左节点无覆盖，右节点覆盖
                # left == 2 && right == 0 左节点覆盖，右节点无覆盖
        if left == 0 or right == 0:
            self.res += 1
            return 1
        # 情况三，左右有一边有摄像头，父节点为有覆盖
								# left == 1 && right == 2 左节点有摄像头，右节点有覆盖
                # left == 2 && right == 1 左节点有覆盖，右节点有摄像头
                # left == 1 && right == 1 左右节点都有摄像头
        if left == 1 or right == 1:
            return 2

    def minCameraCover(self, root: Optional[TreeNode]) -> int:
        # 情况四，如果头节点没有覆盖到，需要加一个摄像头到头节点
        if self.traversal(root) == 0:
            self.res += 1
        return self.res
```

## Greedy 总结

https://programmercarl.com/%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93%E7%AF%87.html