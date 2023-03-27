# Day 28 | 93 Restore IP Addresses | 78, 90 Subsets & II

## 93 Restore IP Addresses

Problem: https://leetcode.com/problems/restore-ip-addresses/description/

Solution: https://www.programmercarl.com/0093.%E5%A4%8D%E5%8E%9FIP%E5%9C%B0%E5%9D%80.html#python

回溯的时候要确定回溯的次数

判断段位是否是有效段位:

- 段位以0为开头的数字不合法
- 段位里有非正整数字符不合法
- 段位如果大于255了不合法

Time complexity: O((M^N)⋅N)

space complexity is O(1)

```python
class Solution:
    def __init__(self):
        self.res = []
        self.path = []

    def restoreIpAddresses(self, s: str):
        self.res.clear()
        self.backtracking(s, 0, 0)
        if len(s) > 12:
            return []
        return self.res

    def is_valid(self, s: str, start: int, end: int) -> bool:
        # 如果start index 大于 i 返回False
        if start > end:
            return False
        # 若数字是0开头,并且不是只有一个0，不合法
        if s[start] == '0' and start != end:
            return False
        # 段位如果大于255了不合法
        if not 0 <= int(s[start:end + 1]) <= 255:
            return False
        return True
    # 需要加一个 point_num 记录递归的次数，最多递归4次
    def backtracking(self, s, start_index, point_num):
        if point_num == 4 and start_index == len(s):
            self.res.append('.'.join(self.path[:]))

        for i in range(start_index, len(s)):
            if self.is_valid(s, start_index, i):
                # [start_index, i + 1]就是被截取的子串
                self.path.append(s[start_index: i + 1])
                self.backtracking(s, i + 1, point_num + 1)
                self.path.pop()
            # 若当前被截取的子串大于255或者大于三位数，直接结束本层循环
            else:
                break
```

## 78 Subsets

Problem: https://leetcode.com/problems/subsets/description/

Solution: https://www.programmercarl.com/0078.%E5%AD%90%E9%9B%86.html#%E5%9B%9E%E6%BA%AF%E4%B8%89%E9%83%A8%E6%9B%B2

回溯，可以不写终止条件， 不会无限递归，因为每次递归的下一层就是从i+1开始的

Time complexity: O(N*2^N)

Space : O(N)

```python
class Solution:
    def __init__(self):
        self.res = []
        self.path = []

    def subsets(self, nums):
        self.res.clear()
        self.path.clear()
        self.backtracking(nums, 0)
        return self.res

    def backtracking(self, nums, start_index):
        self.res.append(self.path[:]) #收集子集，要放在终止添加的上面，否则会漏掉自己
        if start_index == len(nums): #终止条件可以不加
            return
        for i in range(start_index, len(nums)):
            self.path.append(nums[i])
            self.backtracking(nums, i + 1)
            self.path.pop()
```

## 90 Subsets II

Problem: https://leetcode.com/problems/subsets-ii/description/

Solution: https://www.programmercarl.com/0090.%E5%AD%90%E9%9B%86II.html#%E6%80%9D%E8%B7%AF

回溯 + 层级去重：

Time complexity: O(n⋅2^n)

Space complexity: O(n⋅2^n)

```python
class Solution:
    def __init__(self):
        self.paths = []
        self.path = []

    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        self.backtracking(nums, 0)
        return self.paths

    def backtracking(self, nums: List[int], start_index: int) -> None:
        # ps.空集合仍符合要求
        self.paths.append(self.path[:])
        # Base Case
        if start_index == len(nums):
            return
        
        # 单层递归逻辑
        for i in range(start_index, len(nums)):
            if i > start_index and nums[i] == nums[i-1]:
                # 当前后元素值相同时，跳入下一个循环，去重
                continue
            self.path.append(nums[i])
            self.backtracking(nums, i+1)
            self.path.pop()
```