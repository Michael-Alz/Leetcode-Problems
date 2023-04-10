# Day 41 | 343 Integer Break | 96 Unique Binary Search Trees

## 343 Integer Break

Problem: https://leetcode.com/problems/integer-break/

Solution: https://programmercarl.com/0343.%E6%95%B4%E6%95%B0%E6%8B%86%E5%88%86.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

dp[i]：分拆数字i，可以得到的最大乘积为dp[i]。

- 时间复杂度：O(n^2)
- 空间复杂度：O(n)

```python
class Solution:
    def integerBreak(self, n: int) -> int:
        dp = (n + 1) * [0]
        dp[2] = 1
         # 假设对正整数 i 拆分出的第一个正整数是 j（1 <= j < i），则有以下两种方案：
            # 1) 将 i 拆分成 j 和 i−j 的和，且 i−j 不再拆分成多个正整数，此时的乘积是 j * (i-j)
            # 2) 将 i 拆分成 j 和 i−j 的和，且 i−j 继续拆分成多个正整数，此时的乘积是 j * d [i-j]
        for i in range(3, n + 1):
            for j in range(1 , i // 2 + 1):
                dp[i] = max(dp[i], max(j * dp[i - j], j * (i - j)))
        return dp[n]
```

贪心：尽量用多用3

- 时间复杂度：O(n)
- 空间复杂度：O(1)

https://leetcode.com/problems/integer-break/solutions/80689/a-simple-explanation-of-the-math-part-and-a-o-n-solution/?orderBy=most_votes

```python
class Solution(object):
    def integerBreak(self, n):
        if n == 2 or n == 3:
            return n - 1
        if n % 3 == 0:
            return 3**(n/3)
        if n % 3 == 1:
            return 3**(n/3 - 1)*4
        if n % 3 == 2:
            return 3**(n/3)*2
```

## 96 Unique Binary Search Trees

Problem:https://leetcode.com/problems/unique-binary-search-trees/description/

Solution: https://programmercarl.com/0096.%E4%B8%8D%E5%90%8C%E7%9A%84%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#%E6%80%9D%E8%B7%AF

1. 确定dp数组（dp table）以及下标的含义

**dp[i] ： 1到i为节点组成的二叉搜索树的个数为dp[i]**。

也可以理解是i个不同元素节点组成的二叉搜索树的个数为dp[i] ，都是一样的。

以下分析如果想不清楚，就来回想一下dp[i]的定义

2. 确定递推公式

在上面的分析中，其实已经看出其递推关系， dp[i] += dp[以j为头结点左子树节点数量] * dp[以j为头结点右子树节点数量]

j相当于是头结点的元素，从1遍历到i为止。

所以递推公式：dp[i] += dp[j - 1] * dp[i - j]; ，j-1 为j为头结点左子树节点数量，i-j 为以j为头结点右子树节点数量

3. dp数组如何初始化

初始化，只需要初始化dp[0]就可以了，推导的基础，都是dp[0]。

那么dp[0]应该是多少呢？

从定义上来讲，空节点也是一棵二叉树，也是一棵二叉搜索树，这是可以说得通的。

从递归公式上来讲，dp[以j为头结点左子树节点数量] * dp[以j为头结点右子树节点数量] 中以j为头结点左子树节点数量为0，也需要dp[以j为头结点左子树节点数量] = 1， 否则乘法的结果就都变成0了。

所以初始化dp[0] = 1

4. 确定遍历顺序

首先一定是遍历节点数，从递归公式：dp[i] += dp[j - 1] * dp[i - j]可以看出，节点数为i的状态是依靠 i之前节点数的状态。

那么遍历i里面每一个数作为头结点的状态，用j来遍历。

5. 举例推导dp数组: n = 5, 1 1 2 5 14 42

- 时间复杂度：O(n^2)
- 空间复杂度：O(n)

```python
class Solution:
    def numTrees(self, n: int) -> int:
        # dp[i] ： 1到i为节点组成的二叉搜索树的个数为dp[i]。
        dp = [0] * (n + 1)
        dp[0] = 1
        dp[1] = 1
        for i in range(2 , n + 1):
          for j in range(1 , i + 1):
            # 以j为头节点，dp[左边的节点个数]乘以dp[右边的节点个数]
              dp[i] += dp[j - 1] * dp[i - j]
        return dp[n]
```