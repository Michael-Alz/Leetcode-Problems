# Day 58 | 739 Daily Temperatures | 496 Next Greater Element I

## 739 Daily Temperatures

Problem:https://leetcode.com/problems/daily-temperatures/description/

Solution:https://programmercarl.com/0739.%E6%AF%8F%E6%97%A5%E6%B8%A9%E5%BA%A6.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

单调栈使用时机：

**通常是一维数组，要寻找任一个元素的右边或者左边第一个比自己大或者小的元素的位置，此时我们就要想到可以用单调栈了，**时间复杂度为O(n)。

- 情况一：当前遍历的元素T[i]小于栈顶元素T[st.top()]的情况
- 情况二：当前遍历的元素T[i]等于栈顶元素T[st.top()]的情况
- 情况三：当前遍历的元素T[i]大于栈顶元素T[st.top()]的情况
- 时间复杂度：O(n)
- 空间复杂度：O(n)

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:

        # 如果当前遍历的元素 大于栈顶元素，表示 栈顶元素的 右边的最大的元素就是 当前遍历的元素，
        # 所以弹出 栈顶元素，并记录
        # 如果栈不空的话，还要考虑新的栈顶与当前元素的大小关系
        # 否则的话，可以直接入栈。
        # 注意，单调栈里 加入的元素是 下标。

        stack = [0]
        n = len(temperatures)
        res = [0] * n
        for i in range(1, n):
            while stack and temperatures[stack[-1]] < temperatures[i]:
                t = stack.pop()
                res[t] = i - t
            stack.append(i)
        return res
```

## 496 Next Greater Element I

Problem: https://leetcode.com/problems/next-greater-element-i/description/

Solution: https://programmercarl.com/0496.%E4%B8%8B%E4%B8%80%E4%B8%AA%E6%9B%B4%E5%A4%A7%E5%85%83%E7%B4%A0I.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

暴力解法：

- Time complexity: O(m⋅n)
- Space complexity: O(n)

```python
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        index1 = 0
        indexl = []
        res = [-1] * len(nums1)
        index2 = 0
        # 找到nums1在nums2里的下标
        while index1 < len(nums1):
            for i in range(len(nums2)):
                if nums1[index1] == nums2[i]:
                    indexl.append(i)
            index1 += 1
        # 分别找第一个比下标数大的元素
        for i in indexl:
            for j in range(i + 1,len(nums2)):
                if nums2[j] > nums2[i]:
                    res[index2] = nums2[j]
                    break
            index2 += 1
        return res
```

单调栈：

- Time complexity: O(n)
- Space complexity: O(n)

```python
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        st = [0]
        res = [-1] * len(nums1)
        hmap = {}
        # key:下标元素，value：下标
        for i in range(len(nums1)):
            hmap[nums1[i]] = i
        for i in range(1 , len(nums2)):
            if nums2[i] <= nums2[st[-1]]:
                st.append(i)
            else:
                while st and nums2[st[-1]] < nums2[i]:
                    if nums2[st[-1]] in nums1:
                        res[hmap[nums2[st[-1]]]] = nums2[i]
                    st.pop()
                st.append(i)
        return res
```