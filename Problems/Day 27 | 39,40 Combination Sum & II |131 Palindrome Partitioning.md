# Day 27 | 39,40 Combination Sum & II |131 Palindrome Partitioning

## 39 Combination Sum

Problem:https://leetcode.com/problems/combination-sum/description/

Solution:https://www.programmercarl.com/0039.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C.html#python

回溯:

Time Complexity: O(N^((T/M)+1))

Space: O(T/M)

```python
class Solution:
    def __init__(self):
        self.path = []
        self.paths = []

    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        '''
        因为本题没有组合数量限制，所以只要元素总和大于target就算结束
        '''
        self.path.clear()
        self.paths.clear()

        # 为了剪枝需要提前进行排序
        candidates.sort()
        self.backtracking(candidates, target, 0, 0)
        return self.paths

    def backtracking(self, candidates: List[int], target: int, sum_: int, start_index: int) -> None:
        # Base Case
        if sum_ == target:
            self.paths.append(self.path[:]) # 因为是shallow copy，所以不能直接传入self.path
            return
        # 单层递归逻辑
        # 如果本层 sum + condidates[i] > target，就提前结束遍历，剪枝
        for i in range(start_index, len(candidates)):
            if sum_ + candidates[i] > target:
                return
            sum_ += candidates[i]
            self.path.append(candidates[i])
            self.backtracking(candidates, target, sum_, i)  # 因为无限制重复选取，所以不是i-1
            sum_ -= candidates[i]   # 回溯
            self.path.pop()        # 回溯
```

## 40 Combination Sum II

Problem: https://leetcode.com/problems/combination-sum-ii/description/

Solution: https://www.programmercarl.com/0040.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CII.html#python

回溯：需要明白去重逻辑，树层去重

```python
class Solution:
    def __init__(self):
        self.paths = []
        self.path = []

    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        '''
        类似于求三数之和，求四数之和，为了避免重复组合，需要提前进行数组排序
        '''
        self.paths.clear()
        self.path.clear()
        # 必须提前进行数组排序，避免重复
        candidates.sort()
        self.backtracking(candidates, target, 0, 0)
        return self.paths

    def backtracking(self, candidates: List[int], target: int, sum_: int, start_index: int) -> None:
        # Base Case
        if sum_ == target:
            self.paths.append(self.path[:])
            return
        
        # 单层递归逻辑
        for i in range(start_index, len(candidates)):
            # 剪枝，同39.组合总和
            if sum_ + candidates[i] > target:
                return
            
            # 跳过同一树层使用过的元素
            if i > start_index and candidates[i] == candidates[i-1]:
                continue
            
            sum_ += candidates[i]
            self.path.append(candidates[i])
            self.backtracking(candidates, target, sum_, i+1)
            self.path.pop()             # 回溯，为了下一轮for loop
            sum_ -= candidates[i]       # 回溯，为了下一轮for loop
```

使用used数组：

```python
class Solution:
    def __init__(self):
        self.paths = []
        self.path = []
        self.used = []

    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        '''
        类似于求三数之和，求四数之和，为了避免重复组合，需要提前进行数组排序
        本题需要使用used，用来标记区别同一树层的元素使用重复情况：注意区分递归纵向遍历遇到的重复元素，和for循环遇到的重复元素，这两者的区别
        '''
        self.paths.clear()
        self.path.clear()
        self.usage_list = [False] * len(candidates)
        # 必须提前进行数组排序，避免重复
        candidates.sort()
        self.backtracking(candidates, target, 0, 0)
        return self.paths

    def backtracking(self, candidates: List[int], target: int, sum_: int, start_index: int) -> None:
        # Base Case
        if sum_ == target:
            self.paths.append(self.path[:])
            return
        
        # 单层递归逻辑
        for i in range(start_index, len(candidates)):
            # 剪枝，同39.组合总和
            if sum_ + candidates[i] > target:
                return
            
            # 检查同一树层是否出现曾经使用过的相同元素
            # 若数组中前后元素值相同，但前者却未被使用(used == False)，说明是for loop中的同一树层的相同元素情况
            if i > 0 and candidates[i] == candidates[i-1] and self.usage_list[i-1] == False:
                continue

            sum_ += candidates[i]
            self.path.append(candidates[i])
            self.usage_list[i] = True
            self.backtracking(candidates, target, sum_, i+1)
            self.usage_list[i] = False  # 回溯，为了下一轮for loop
            self.path.pop()             # 回溯，为了下一轮for loop
            sum_ -= candidates[i]       # 回溯，为了下一轮for loop
```

## 131 Palindrome Partitioning

Problem: https://leetcode.com/problems/palindrome-partitioning/description/

Solution: https://www.programmercarl.com/0131.%E5%88%86%E5%89%B2%E5%9B%9E%E6%96%87%E4%B8%B2.html#python

回溯，用start index作为隔板:

Time Complexity : O(N⋅2^N)

Space Complexity: O(N)

```python
#回溯+函数判断回文串
class Solution:
    def __init__(self):
        self.paths = []
        self.path = []

    def partition(self, s: str) -> List[List[str]]:
        '''
        递归用于纵向遍历
        for循环用于横向遍历
        当切割线迭代至字符串末尾，说明找到一种方法
        类似组合问题，为了不重复切割同一位置，需要start_index来做标记下一轮递归的起始位置(切割线)
        '''
        self.path.clear()
        self.paths.clear()
        self.backtracking(s, 0)
        return self.paths

    def backtracking(self, s: str, start_index: int) -> None:
        # Base Case
        if start_index >= len(s):
            self.paths.append(self.path[:])
            return
        
        # 单层递归逻辑
        for i in range(start_index, len(s)):
            # 此次比其他组合题目多了一步判断：
            # 判断被截取的这一段子串([start_index, i])是否为回文串
            if self.is_palindrome(s, start_index, i):
                self.path.append(s[start_index:i+1])
                self.backtracking(s, i+1)   # 递归纵向遍历：从下一处进行切割，判断其余是否仍为回文串
                self.path.pop()             # 回溯
            else:
                continue    

    def is_palindrome(self, s: str, start: int, end: int) -> bool:
        i: int = start        
        j: int = end
        while i < j:
            if s[i] != s[j]:
                return False
            i += 1
            j -= 1
        return True
#回溯+正反序判断回文串
class Solution:
    def __init__(self):
        self.paths = []
        self.path = []

    def partition(self, s: str) -> List[List[str]]:
        '''
        递归用于纵向遍历
        for循环用于横向遍历
        当切割线迭代至字符串末尾，说明找到一种方法
        类似组合问题，为了不重复切割同一位置，需要start_index来做标记下一轮递归的起始位置(切割线)
        '''
        self.path.clear()
        self.paths.clear()
        self.backtracking(s, 0)
        return self.paths

    def backtracking(self, s: str, start_index: int) -> None:
        # Base Case
        if start_index >= len(s):
            self.paths.append(self.path[:])
            return
        
        # 单层递归逻辑
        for i in range(start_index, len(s)):
            # 此次比其他组合题目多了一步判断：
            # 判断被截取的这一段子串([start_index, i])是否为回文串
            temp = s[start_index:i+1]
            if temp == temp[::-1]:  # 若反序和正序相同，意味着这是回文串
                self.path.append(temp)
                self.backtracking(s, i+1)   # 递归纵向遍历：从下一处进行切割，判断其余是否仍为回文串
                self.path.pop()
            else:
                continue
```

