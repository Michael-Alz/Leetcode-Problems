# Day 52 | 300 Longest Increasing Subsequence | 674 Longest Continuous Increasing Subsequence | 718 Maximum Length of Repeated Subarray

## 300 Longest Increasing Subsequence

Problem:https://leetcode.com/problems/longest-increasing-subsequence/description/

Solution:https://programmercarl.com/0300.%E6%9C%80%E9%95%BF%E4%B8%8A%E5%8D%87%E5%AD%90%E5%BA%8F%E5%88%97.html#%E6%80%9D%E8%B7%AF

- dp[i]的定义

本题中，正确定义dp数组的含义十分重要。

**dp[i]表示i之前包括i的以nums[i]结尾的最长递增子序列的长度**

为什么一定表示 “以nums[i]结尾的最长递增子序” ，因为我们在 做 递增比较的时候，如果比较 nums[j] 和 nums[i] 的大小，那么两个递增子序列一定分别以nums[j]为结尾 和 nums[i]为结尾， 要不然这个比较就没有意义了，不是尾部元素的比较那么 如何算递增呢。

- 状态转移方程

位置i的最长升序子序列等于j从0到i-1各个位置的最长升序子序列 + 1 的最大值。

所以：if (nums[i] > nums[j]) dp[i] = max(dp[i], dp[j] + 1);

**注意这里不是要dp[i] 与 dp[j] + 1进行比较，而是我们要取dp[j] + 1的最大值**。

- dp[i]的初始化

每一个i，对应的dp[i]（即最长递增子序列）起始大小至少都是1.

- 确定遍历顺序

dp[i] 是有0到i-1各个位置的最长递增子序列 推导而来，那么遍历i一定是从前向后遍历。

j其实就是遍历0到i-1，那么是从前到后，还是从后到前遍历都无所谓，只要吧 0 到 i-1 的元素都遍历了就行了。 所以默认习惯 从前向后遍历。

- Time complexity: O(N^2)
- Space complexity: O(N)

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        # dp[i]表示i之前包括i的以nums[i]结尾的最长递增子序列的长度
        dp = [1] * (len(nums))
        for i in range(1 , len(dp)):
            for j in range(0, i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i],dp[j] + 1 )
        return dp[-1]
```

## 674 Longest Continuous Increasing Subsequence

Problem:https://leetcode.com/problems/longest-continuous-increasing-subsequence/description/

Solution:https://programmercarl.com/0674.%E6%9C%80%E9%95%BF%E8%BF%9E%E7%BB%AD%E9%80%92%E5%A2%9E%E5%BA%8F%E5%88%97.html#%E6%80%9D%E8%B7%AF

dp:

Time: O(N)

Space: O(N)

```python
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        # dp[i]：以下标i为结尾的连续递增的子序列长度为dp[i]
        # 注意这里的定义，一定是以下标i为结尾，并不是说一定以下标0为起始位置。
        dp = [1] * len(nums)
        for i in range(1, len(nums)):
            if nums[i] > nums[i - 1]:
                dp[i] = max(dp[i], dp[i - 1] + 1 )
        return max(dp)
```

Time: O(N)

Space: O(1)

```python
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        cnt = 1
        m = 1
        for i in range(1, len(nums)):
            if nums[i - 1] < nums[i]:
                cnt += 1
            else:
                m = max(m,cnt)
                cnt = 1
        return max(cnt,m)
```

## 718 Maximum Length of Repeated Subarray

Problem: https://leetcode.com/problems/maximum-length-of-repeated-subarray/description/

Solution: https://programmercarl.com/0718.%E6%9C%80%E9%95%BF%E9%87%8D%E5%A4%8D%E5%AD%90%E6%95%B0%E7%BB%84.html#%E6%80%9D%E8%B7%AF

- 确定dp数组（dp table）以及下标的含义

dp[i][j] ：以下标i - 1为结尾的A，和以下标j - 1为结尾的B，最长重复子数组长度为dp[i][j]。 （**特别注意**： “以下标i - 1为结尾的A” 标明一定是 以A[i-1]为结尾的字符串 ）

此时细心的同学应该发现，那dp[0][0]是什么含义呢？总不能是以下标-1为结尾的A数组吧。

其实dp[i][j]的定义也就决定着，我们在遍历dp[i][j]的时候i 和 j都要从1开始。

那有同学问了，我就定义dp[i][j]为 以下标i为结尾的A，和以下标j 为结尾的B，最长重复子数组长度。不行么？

行倒是行！ 但实现起来就麻烦一点，需要单独处理初始化部分，在本题解下面的拓展内容里，我给出了 第二种 dp数组的定义方式所对应的代码和讲解，大家比较一下就了解了。

- 确定递推公式

根据dp[i][j]的定义，dp[i][j]的状态只能由dp[i - 1][j - 1]推导出来。

即当A[i - 1] 和B[j - 1]相等的时候，dp[i][j] = dp[i - 1][j - 1] + 1;

根据递推公式可以看出，遍历i 和 j 要从1开始！

- dp数组如何初始化

根据dp[i][j]的定义，dp[i][0] 和dp[0][j]其实都是没有意义的！

但dp[i][0] 和dp[0][j]要初始值，因为 为了方便递归公式dp[i][j] = dp[i - 1][j - 1] + 1;

所以dp[i][0] 和dp[0][j]初始化为0。

举个例子A[0]如果和B[0]相同的话，dp[1][1] = dp[0][0] + 1，只有dp[0][0]初始为0，正好符合递推公式逐步累加起来。

- 确定遍历顺序

外层for循环遍历A，内层for循环遍历B。

那又有同学问了，外层for循环遍历B，内层for循环遍历A。不行么？

也行，一样的，我这里就用外层for循环遍历A，内层for循环遍历B了。

同时题目要求长度最长的子数组的长度。所以在遍历的时候顺便把dp[i][j]的最大值记录下来。

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e2c6e45d-642c-444b-8cf2-86d0389d8798/Untitled.png)

- 时间复杂度：O(n × m)，n 为A长度，m为B长度
- 空间复杂度：O(n × m)

```python
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        # dp[i][j] ：以下标i - 1为结尾的A，和以下标j - 1为结尾的B，最长重复子数组长度为dp[i][j]。 （特别注意： “以下标i - 1为结尾的A” 标明一定是 以A[i-1]为结尾的字符串 ）
        n = len(nums1)
        m = len(nums2)
        dp = [[0] * (m + 1) for _ in range(n + 1)]
        for i in range(1 , n + 1):
            for j in range(1 , m + 1):
                if nums1[i - 1] == nums2[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1] + 1
        
        # The map() function executes a specified function for each item in an          iterable. The item is sent to the function as a parameter.
        return max(map(max,dp))

# 时间复杂度：O(n × m)，n 为A长度，m为B长度
# 空间复杂度：O(m)
class Solution:
    def findLength(self, A: List[int], B: List[int]) -> int:
        dp = [0] * (len(B) + 1)
        result = 0
        for i in range(1, len(A)+1):
            for j in range(len(B), 0, -1):
                if A[i-1] == B[j-1]:
                    dp[j] = dp[j-1] + 1
                else:
                    dp[j] = 0 #注意这里不相等的时候要有赋0的操作
                result = max(result, dp[j])
        return result
```