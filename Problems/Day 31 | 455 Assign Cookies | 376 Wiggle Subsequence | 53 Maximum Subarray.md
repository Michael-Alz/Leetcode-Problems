# Day 31 | 455 Assign Cookies | 376 Wiggle Subsequence | 53 Maximum Subarray

## 455 Assign Cookies

Problem: https://leetcode.com/problems/assign-cookies/description/

Solution: https://programmercarl.com/0455.%E5%88%86%E5%8F%91%E9%A5%BC%E5%B9%B2.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

优先考虑大胃口：

Time: O(nlogn)

```python
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        g.sort()
        s.sort()
        start, count = len(s) - 1, 0
        for index in range(len(g) - 1, -1, -1): # 先喂饱大胃口
            if start >= 0 and g[index] <= s[start]:
                start -= 1
                count += 1
        return count
```

优先考虑小胃口：

```python
class Solution:
    # 思路1：优先考虑小胃口
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        g.sort()
        s.sort()
        res = 0
        for i in range(len(s)):
            if res <len(g) and s[i] >= g[res]:  #小饼干先喂饱小胃口
                res += 1
        return res
```

## 376 Wiggle Subsequence

Problem: https://leetcode.com/problems/wiggle-subsequence/

Solution: https://programmercarl.com/0376.%E6%91%86%E5%8A%A8%E5%BA%8F%E5%88%97.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

考虑三种情况：

1. 情况一：上下坡中有平坡
2. 情况二：数组首尾两端
3. 情况三：单调坡中有平坡

- Time complexity : O(n)
- Space complexity : O(1)

```python
class Solution:
    def wiggleMaxLength(self, nums: List[int]) -> int:
        #题目里nums长度大于等于1，当长度为1时，其实到不了for循环里去，所以不用考虑nums长度
        pre = 0
        cur = 0
        res = 1
        for i in range(len(nums) -1):
            cur = nums[i + 1] - nums[i]
            # 差值为0时，不算摆动
            if cur * pre =< 0 and cur != 0 :
                res += 1
                #如果当前差值和上一个差值为一正一负时，才需要用当前差值替代上一个差值
                pre = cur
        return res
```

## 53 Maximum Subarray

Problem: https://leetcode.com/problems/maximum-subarray/description/

Solution: https://programmercarl.com/0053.%E6%9C%80%E5%A4%A7%E5%AD%90%E5%BA%8F%E5%92%8C.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

局部最优：当前“连续和”为负数的时候立刻放弃，从下一个元素重新计算“连续和”，因为负数加上下一个元素 “连续和”只会越来越小。

全局最优：选取最大“连续和”

Time complexity: O(N)

Space complexity: O(1)

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        res = -float('inf')
        count = 0
        for i in range(len(nums)):
            count += nums[i]
            # 取区间累计的最大值（相当于不断确定最大子序终止位置）
            if count > res:
                res = count
            # 相当于重置最大子序起始位置，因为遇到负数一定是拉低总和
            if count <=  0:
                count = 0
        return res
```