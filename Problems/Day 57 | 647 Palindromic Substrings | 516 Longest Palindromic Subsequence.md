# Day 57 | 647 Palindromic Substrings | 516 Longest Palindromic Subsequence

## 647 Palindromic Substrings

Problem:https://leetcode.com/problems/palindromic-substrings/

Solution:https://programmercarl.com/0647.%E5%9B%9E%E6%96%87%E5%AD%90%E4%B8%B2.html#%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92

1. dp数组含义

   布尔类型的dp[i][j]：表示区间范围[i,j] （注意是左闭右闭）的子串是否是回文子串，如果是dp[i][j]为true，否则为false。

2. 在确定递推公式时，就要分析如下几种情况。

   整体上是两种，就是s[i]与s[j]相等，s[i]与s[j]不相等这两种。

   当s[i]与s[j]不相等，那没啥好说的了，dp[i][j]一定是false。

   当s[i]与s[j]相等时，这就复杂一些了，有如下三种情况

   - 情况一：下标i 与 j相同，同一个字符例如a，当然是回文子串
   - 情况二：下标i 与 j相差为1，例如aa，也是回文子串
   - 情况三：下标：i 与 j相差大于1的时候，例如cabac，此时s[i]与s[j]已经相同了，我们看i到j区间是不是回文子串就看aba是不是回文就可以了，那么aba的区间就是 i+1 与 j-1区间，这个区间是不是回文就看dp[i + 1][j - 1]是否为true。

   result就是统计回文子串的数量。

   注意这里我没有列出当s[i]与s[j]不相等的时候，因为在下面dp[i][j]初始化的时候，就初始为false。

3. dp[i][j]初始化为false

4. 首先从递推公式中可以看出，情况三是根据dp[i + 1][j - 1]是否为true，在对dp[i][j]进行赋值true的。**所以一定要从下到上，从左到右遍历，这样保证dp[i + 1][j - 1]都是经过计算的.**

- Time Complexity: O(N^2)
- Space Complexity: O(N^2)

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        # 布尔类型的dp[i][j]：表示区间范围[i,j] （注意是左闭右闭）的子串是否是回文子串，如果是dp[i][j]为true，否则为false。
        n = len(s)
        res = 0
        dp = [[False] * n for _ in range(n)]
        for i in range(n - 1, -1 ,-1):
            for j in range(i, n):
                if s[i] == s[j]:
                    if j - i <= 1:
                        res += 1
                        dp[i][j] = True
                    elif dp[i + 1][j - 1]:
                        res += 1
                        dp[i][j] == True
        return res
```

## 516 Longest Palindromic Subsequence

Problem:https://leetcode.com/problems/longest-palindromic-subsequence/

Solution:https://programmercarl.com/0516.%E6%9C%80%E9%95%BF%E5%9B%9E%E6%96%87%E5%AD%90%E5%BA%8F%E5%88%97.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

1. **dp[i][j]：字符串s在[i, j]范围内最长的回文子序列的长度为dp[i][j]**

2. 在判断回文子串的题目中，关键逻辑就是看s[i]与s[j]是否相同。

   如果s[i]与s[j]相同，那么dp[i][j] = dp[i + 1][j - 1] + 2;

   如果s[i]与s[j]不相同，说明s[i]和s[j]的同时加入 并不能增加[i,j]区间回文子序列的长度，那么分别加入s[i]、s[j]看看哪一个可以组成最长的回文子序列。

   加入s[j]的回文子序列长度为dp[i + 1][j]。

   加入s[i]的回文子序列长度为dp[i][j - 1]。

   那么dp[i][j]一定是取最大的，即：dp[i][j] = max(dp[i + 1][j], dp[i][j - 1]);

3. 首先要考虑当i 和j 相同的情况，从递推公式：dp[i][j] = dp[i + 1][j - 1] + 2; 可以看出 递推公式是计算不到 i 和j相同时候的情况。

   所以需要手动初始化一下，当i与j相同，那么dp[i][j]一定是等于1的，即：一个字符的回文子序列长度就是1。

   其他情况dp[i][j]初始为0就行，这样递推公式：dp[i][j] = max(dp[i + 1][j], dp[i][j - 1]); 中dp[i][j]才不会被初始值覆盖。

4. 从递归公式中，可以看出，dp[i][j] 依赖于 dp[i + 1][j - 1] ，dp[i + 1][j] 和 dp[i][j - 1]

   **所以遍历i的时候一定要从下到上遍历，这样才能保证下一行的数据是经过计算的**。

   j的话，可以正常从左向右遍历。

- Time complexity: O(n^2)
- Space complexity: O(n^2)

```python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        # dp[i][j]：字符串s在[i, j]范围内最长的回文子序列的长度为dp[i][j]。
        n = len(s)
        dp = [[0] * n for _ in range(n)]
        res = 0
        for i in range(n):
            dp[i][i] = 1
        for i in range(n - 1 , -1 , -1):
            for j in range(i + 1 , n):
                if s[i] == s[j]:
                    dp[i][j] = dp[i + 1][j - 1] + 2
                else:
                    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])

        return dp[0][-1]
```

## 动态规划总结

动规五部曲分别为：

1. 确定dp数组（dp table）以及下标的含义
2. 确定递推公式
3. dp数组如何初始化
4. 确定遍历顺序
5. 举例推导dp数组

![img](https://kstar-1253855093.cos.ap-nanjing.myqcloud.com/baguwenpdf/_%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E6%80%9D%E7%BB%B4%E5%AF%BC%E5%9B%BE_%E9%9D%92.png)