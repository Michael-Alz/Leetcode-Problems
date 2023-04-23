# Day 53 | 1143 Longest Common Subsequence | 1035  Uncrossed Lines | 53 Maximum Subarray

## 1143 Longest Common Subsequence

Problem: https://leetcode.com/problems/longest-common-subsequence/description/

Solution:https://programmercarl.com/1143.%E6%9C%80%E9%95%BF%E5%85%AC%E5%85%B1%E5%AD%90%E5%BA%8F%E5%88%97.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

https://leetcode.com/problems/longest-common-subsequence/editorial/

- Time complexity : O(M⋅N)
- Space complexity : O(M⋅N)

defination:

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, `"ace"` is a subsequence of `"abcde"`.

A **common subsequence** of two strings is a subsequence that is common to both strings.

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        # DP[i][j] represents the longest common subsequence of text1[0 ... i] & text2[0 ... j]
        m = len(text1)
        n = len(text2)
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(1 , m + 1):
            for j in range(1 , n + 1):
                if text1[i - 1] == text2[j - 1]:
                    # 每一行只能匹配一次
                    dp[i][j] = dp[i - 1][j - 1] + 1
                else:
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])     
        return dp[-1][-1]
```

- Time complexity : O(M⋅N)
- Space complexity : O(min⁡(M,N))

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        
        # If text1 doesn't reference the shortest string, swap them.
        if len(text2) < len(text1):
            text1, text2 = text2, text1
        
        
        # The previous and current column starts with all 0's and like 
        # before is 1 more than the length of the first word.
        previous = [0] * (len(text1) + 1)
        current = [0] * (len(text1) + 1)
        
        # Iterate up each column, starting from the last one.
        for col in reversed(range(len(text2))):
            for row in reversed(range(len(text1))):
                if text2[col] == text1[row]:
                    current[row] = 1 + previous[row + 1]
                else:
                    current[row] = max(previous[row], current[row + 1])
            # The current column becomes the previous one, and vice versa.
            previous, current = current, previous
        
        # The original problem's answer is in previous[0]. Return it.
        return previous[0]
```

## 1035  Uncrossed Lines

Problem: https://leetcode.com/problems/uncrossed-lines/description/

Solution: https://programmercarl.com/1035.%E4%B8%8D%E7%9B%B8%E4%BA%A4%E7%9A%84%E7%BA%BF.html#%E6%80%9D%E8%B7%AF

**本题说是求绘制的最大连线数，其实就是求两个字符串的最长公共子序列的长度.**

[**Longest Common Subsequence**](https://www.notion.so/Longest-Common-Subsequence-555c3521a3dd4b8689ff65ee8d17493e)

- Time complexity: O(m⋅n)
- Space complexity: O(m⋅n)

```python
class Solution:
    def maxUncrossedLines(self, nums1: List[int], nums2: List[int]) -> int:
        # 本质上是求最长公共子序列（相对顺序不变）
        m = len(nums1) + 1
        n = len(nums2) + 1
        dp = [[0] * n for _ in range(m)]
        for i in range(1, m):
            for j in range(1 , n):
                if nums1[i - 1] == nums2[j - 1]:
                    dp[i][j] = dp[i -1][j - 1] + 1
                else:
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
        return dp[-1][-1]
```

## 53 Maximum Subarray

Problem: https://leetcode.com/problems/maximum-subarray/description/

Solution: https://programmercarl.com/0053.%E6%9C%80%E5%A4%A7%E5%AD%90%E5%BA%8F%E5%92%8C%EF%BC%88%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%EF%BC%89.html#%E6%80%9D%E8%B7%AF

- 时间复杂度：O(n)
- 空间复杂度：O(n)

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        # dp[i]：包括下标i（以nums[i]为结尾）的最大连续子序列和为dp[i]。
        n = len(nums)
        dp = [0] * n
        dp[0] = nums[0]
        for i in range(1 , n):
            #dp[i]只有两个方向可以推出来：
            # dp[i - 1] + nums[i]，即：nums[i]加入当前连续子序列和
            # nums[i]，即：从头开始计算当前连续子序列和
            # 一定是取最大的，所以dp[i] = max(dp[i - 1] + nums[i], nums[i]);
            dp[i] = max(dp[i - 1] + nums[i], nums[i])
        return max(dp)
```