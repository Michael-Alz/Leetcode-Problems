# Day 55 | 392 Is Subsequence | 115 Distinct Subsequences

## 392 Is Subsequence

Problem: https://leetcode.com/problems/is-subsequence/

Solution: https://programmercarl.com/0392.%E5%88%A4%E6%96%AD%E5%AD%90%E5%BA%8F%E5%88%97.html

双指针：

- Time Complexity: O(∣T∣)
- Space Complexity: O(1)

```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        LEFT_BOUND, RIGHT_BOUND = len(s), len(t)
        p_left = p_right = 0
        while p_left < LEFT_BOUND and p_right < RIGHT_BOUND:
            # move both pointers or just the right pointer
            if s[p_left] == t[p_right]:
                p_left += 1
            p_right += 1

        return p_left == LEFT_BOUND
```

dp:

- Time Complexity: O(∣S∣⋅∣T∣)
- Space Complexity: O(∣S∣⋅∣T∣)

**dp[i][j] 表示以下标i-1为结尾的字符串s，和以下标j-1为结尾的字符串t，相同子序列的长度为dp[i][j]**。

```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        m , n = len(s) + 1, len(t) + 1
        dp = [[0] * n for _ in range(m)]
        for i in range(1, m):
            for j in range(1 , n):
                if s[i - 1] == t[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1] + 1
                else:
                    dp[i][j] = dp[i][j - 1]
        return dp[-1][-1] == m - 1
```

## 115 Distinct Subsequences

Problem: https://leetcode.com/problems/distinct-subsequences/description/

Solution: https://programmercarl.com/0115.%E4%B8%8D%E5%90%8C%E7%9A%84%E5%AD%90%E5%BA%8F%E5%88%97.html#%E6%80%9D%E8%B7%AF

1. 确定dp数组（dp table）以及下标的含义

   dp[i][j]：以i-1为结尾的s子序列中出现以j-1为结尾的t的个数为dp[i][j]。

2. 确定递推公式

   这一类问题，基本是要分析两种情况

   - s[i - 1] 与 t[j - 1]相等
   - s[i - 1] 与 t[j - 1] 不相等

   当s[i - 1] 与 t[j - 1]相等时，dp[i][j]可以有两部分组成。

   一部分是用s[i - 1]来匹配，那么个数为dp[i - 1][j - 1]。即不需要考虑当前s子串和t子串的最后一位字母，所以只需要 dp[i-1][j-1]。

   一部分是不用s[i - 1]来匹配，个数为dp[i - 1][j]。

   例如： s：bagg 和 t：bag ，s[3] 和 t[2]是相同的，但是字符串s也可以不用s[3]来匹配，即用s[0]s[1]s[2]组成的bag。

   当然也可以用s[3]来匹配，即：s[0]s[1]s[3]组成的bag。

   所以当s[i - 1] 与 t[j - 1]相等时，dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];

   当s[i - 1] 与 t[j - 1]不相等时，dp[i][j]只有一部分组成，不用s[i - 1]来匹配（就是模拟在s中删除这个元素），即：dp[i - 1][j]

   所以递推公式为：dp[i][j] = dp[i - 1][j];

   这里可能有录友还疑惑，为什么只考虑 “不用s[i - 1]来匹配” 这种情况， 不考虑 “不用t[j - 1]来匹配” 的情况呢。

   这里大家要明确，我们求的是 s 中有多少个 t，而不是 求t中有多少个s，所以只考虑 s中删除元素的情况，即 不用s[i - 1]来匹配 的情况。

3. 从递推公式dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]; 和 dp[i][j] = dp[i - 1][j]; 中可以看出dp[i][j] 是从上方和左上方推导而来, 那么 dp[i][0] 和dp[0][j]是一定要初始化的。

   dp[i][0] 表示：以i-1为结尾的s可以随便删除元素，出现空字符串的个数。

   那么dp[i][0]一定都是1，因为也就是把以i-1为结尾的s，删除所有元素，出现空字符串的个数就是1。

   再来看dp[0][j]，dp[0][j]：空字符串s可以随便删除元素，出现以j-1为结尾的字符串t的个数。

   那么dp[0][j]一定都是0，s如论如何也变成不了t。

   最后就要看一个特殊位置了，即：dp[0][0] 应该是多少。

   dp[0][0]应该是1，空字符串s，可以删除0个元素，变成空字符串t。

4. 从递推公式dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]; 和 dp[i][j] = dp[i - 1][j]; 中可以看出dp[i][j]都是根据左上方和正上方推出来的。所以遍历的时候一定是从上到下，从左到右，这样保证dp[i][j]可以根据之前计算出来的数值进行计算。

- Time Complexity:  O(M×N)
- Space Complexity: O(M×N)

```python
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        # dp[i][j]：以i-1为结尾的s子序列中出现以j-1为结尾的t的个数为dp[i][j]。
        m , n = len(s) + 1, len(t) + 1
        dp = [[0] * n for _ in range(m)]
        for i in range(m - 1):
            dp[i][0] = 1
        for i in range(1 , m):
            for j in range(1 , n):
                if t[j - 1] == s[i - 1]:
                    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]
                else:
                    dp[i][j] = dp[i - 1][j]
        return dp[-1][-1]
```

- Time Complexity:  O(M×N)
- Space Complexity: O(N)

```python
class SolutionDP2:
    """
    既然dp[i]只用到dp[i - 1]的状态，
    我们可以通过缓存dp[i - 1]的状态来对dp进行压缩，
    减少空间复杂度。
    （原理等同同于滚动数组）
    """
    
    def numDistinct(self, s: str, t: str) -> int:
        n1, n2 = len(s), len(t)
        if n1 < n2:
            return 0

        dp = [0 for _ in range(n2 + 1)]
        dp[0] = 1

        for i in range(1, n1 + 1):
            # 必须深拷贝
            # 不然prev[i]和dp[i]是同一个地址的引用
            prev = dp.copy()
            # 剪枝，保证s的长度大于等于t
            # 因为对于任意i，i > n1, dp[i] = 0
            # 没必要跟新状态。 
            end = i if i < n2 else n2
            for j in range(1, end + 1):
                if s[i - 1] == t[j - 1]:
                    dp[j] = prev[j - 1] + prev[j]
                else:
                    dp[j] = prev[j]
        return dp[-1]
```

