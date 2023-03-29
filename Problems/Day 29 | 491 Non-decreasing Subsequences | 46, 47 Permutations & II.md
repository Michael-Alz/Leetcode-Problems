# Day 29 | 491 Non-decreasing Subsequences | 46, 47 Permutations & II

## 491 Non-decreasing Subsequences

Problem: https://programmercarl.com/0491.%E9%80%92%E5%A2%9E%E5%AD%90%E5%BA%8F%E5%88%97.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

Solution: https://programmercarl.com/0491.%E9%80%92%E5%A2%9E%E5%AD%90%E5%BA%8F%E5%88%97.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

回溯，不能进行排序去重，需要用used set去重

Time : O (2^n*n)

Space: O (2^n*n)

```python
class Solution:
    def __init__(self):
        self.paths = []
        self.path = []

    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        '''
        本题求自增子序列，所以不能改变原数组顺序
        '''
        self.backtracking(nums, 0)
        return self.paths

    def backtracking(self, nums: List[int], start_index: int):
        # 收集结果，同78.子集，仍要置于终止条件之前
        if len(self.path) >= 2:
            # 本题要求所有的节点
            self.paths.append(self.path[:])
        
        # Base Case（可忽略）
        if start_index == len(nums):
            return

        # 单层递归逻辑
        # 深度遍历中每一层都会有一个全新的usage_list用于记录本层元素是否重复使用
        usage_list = set()
        # 同层横向遍历
        for i in range(start_index, len(nums)):
            # 若当前元素值小于前一个时（非递增）或者曾用过，跳入下一循环
            if (self.path and nums[i] < self.path[-1]) or nums[i] in usage_list:
                continue
            usage_list.add(nums[i])
            self.path.append(nums[i])
            self.backtracking(nums, i+1)
            self.path.pop()
```

## 46 Permutations

Problem: https://leetcode.com/problems/permutations/description/

Solution: https://programmercarl.com/0046.%E5%85%A8%E6%8E%92%E5%88%97.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

**回溯+丢掉usage_list:**

Time: O(sum 1 to N (P(N, k))

Space: O(N !)

层序去重，但需要之前的数字，所以用path来判断是否本层用过的数字

```python
class Solution:
    def __init__(self):
        self.path = []
        self.paths = []

    def permute(self, nums: List[int]) -> List[List[int]]:
        '''
        因为本题排列是有序的，这意味着同一层的元素可以重复使用，但同一树枝上不能重复使用
        所以处理排列问题每层都需要从头搜索，故不再使用start_index
        '''
        self.backtracking(nums)
        return self.paths

    def backtracking(self, nums: List[int]) -> None:
        # Base Case本题求叶子节点
        if len(self.path) == len(nums):
            self.paths.append(self.path[:])
            return

        # 单层递归逻辑
        for i in range(0, len(nums)):  # 从头开始搜索
            # 若遇到self.path里已收录的元素，跳过
            if nums[i] in self.path:
                continue
            self.path.append(nums[i])
            self.backtracking(nums)
            self.path.pop()
# 用hashtable去重
class Solution:
    def __init__(self):
        self.path = []
        self.paths = []

    def permute(self, nums: List[int]) -> List[List[int]]:
        '''
        因为本题排列是有序的，这意味着同一层的元素可以重复使用，但同一树枝上不能重复使用(usage_list)
        所以处理排列问题每层都需要从头搜索，故不再使用start_index
        '''
        usage_list = [False] * len(nums)
        self.backtracking(nums, usage_list)
        return self.paths

    def backtracking(self, nums: List[int], usage_list: List[bool]) -> None:
        # Base Case本题求叶子节点
        if len(self.path) == len(nums):
            self.paths.append(self.path[:])
            return

        # 单层递归逻辑
        for i in range(0, len(nums)):  # 从头开始搜索
            # 若遇到self.path里已收录的元素，跳过
            if usage_list[i] == True:
                continue
            usage_list[i] = True
            self.path.append(nums[i])
            self.backtracking(nums, usage_list)     # 纵向传递使用信息，去重
            self.path.pop()
            usage_list[i] = False
```

## 47 Permutations II

Problem: https://leetcode.com/problems/permutations-ii/description/

Solution: https://programmercarl.com/0047.%E5%85%A8%E6%8E%92%E5%88%97II.html#%E6%80%9D%E8%B7%AF

回溯，需要用used去重，有两重去重逻辑：

Time: O(sum 1 to N (P(N, k))

Space: O(N)

```python
class Solution:
    def __init__(self):
        self.res = []
        self.path = []

    def permuteUnique(self, nums):
        nums.sort()
        used = [0] * len(nums)
        self.backtracking(nums, used)  # used数组要输入
        return self.res

    def backtracking(self, nums, used):
        if len(self.path) == len(nums):
            self.res.append(self.path[:])
            return
        for i in range(0, len(nums)):
            if (i > 0 and nums[i - 1] == nums[i]) and (used[i - 1] == 0):  # 层级去重
                continue
            if used[i] == 0:  # 判断下一层不取同一个数字
                used[i] = 1
                self.path.append(nums[i])
                self.backtracking(nums, used)
                self.path.pop()
                used[i] = 0
```