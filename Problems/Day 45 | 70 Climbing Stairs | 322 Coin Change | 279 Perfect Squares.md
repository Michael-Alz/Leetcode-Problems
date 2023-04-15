# Day 45 | 70 Climbing Stairs | 322 Coin Change | 279 Perfect Squares

## 70 Climbing Stairs

Problem: https://leetcode.com/problems/climbing-stairs/description/

Solution: https://programmercarl.com/0070.%E7%88%AC%E6%A5%BC%E6%A2%AF.html#%E6%80%9D%E8%B7%AF

完全01背包：

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        # dp[i]：爬到有i个台阶的楼顶，有dp[i]种方法。
        dp = [0] * (n + 1)
        dp[0] = 1
        # 物品，因为只能走一步或者两步
        m = 2
        # 遍历背包
        for i in range(n + 1):
            # 遍历物品
            for j in range(1 , m + 1):
                if i >= j:
                    # 当前台阶有dp[i - j]种方法（往后退一步或者两步），再进行累加
                    dp[i] += dp[i - j]
        return dp[n]
```

## 322 Coin Change

Problem:https://leetcode.com/problems/coin-change/description/

Solution:https://programmercarl.com/0322.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

- Time complexity : O(S∗n)
- Space complexity : O(S)

递推公式：

凑足总额为j - coins[i]的最少个数为dp[j - coins[i]]，那么只需要加上一个钱币coins[i]即dp[j - coins[i]] + 1就是dp[j]（考虑coins[i]）

所以dp[j] 要取所有 dp[j - coins[i]] + 1 中最小的。

递推公式：dp[j] = min(dp[j - coins[i]] + 1, dp[j]);

遍历顺序：

本题求钱币最小个数，**那么钱币有顺序和没有顺序都可以，都不影响钱币的最小个数**。

所以本题并不强调集合是组合还是排列。

**如果求组合数就是外层for循环遍历物品，内层for遍历背包**。

**如果求排列数就是外层for遍历背包，内层for循环遍历物品**。

**所以本题的两个for循环的关系是：外层for循环遍历物品，内层for遍历背包或者外层for遍历背包，内层for循环遍历物品都是可以的！**

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        # dp[j]：凑足总额为j所需钱币的最少个数为dp[j]
        # 因为需要求最小的，用min的时候初始化需要用一个最大的数字
        dp = [float("inf")] * (amount + 1)
        # 题中给了dp[0]的值
        dp[0] = 0
        # 遍历物品
        for i in range(len(coins)):
            # 遍历背包
            for j in range(coins[i] , amount + 1):
                dp[j] = min(dp[j], dp[j - coins[i]] + 1)
        return dp[amount] if dp[amount] != float("inf") else -1

# 版本二
def coinChange1(self, coins: List[int], amount: int) -> int:
        '''版本二'''
        # 初始化
        dp = [float("inf")]*(amount + 1)
        dp[0] = 0
        # 遍历背包
        for j in range(1, amount + 1):
            # 遍历物品
            for coin in coins:
                if j >= coin:
                	dp[j] = min(dp[j], dp[j - coin] + 1)
        return dp[amount] if dp[amount] != float("inf") else -1
```

## 279 Perfect Squares

Problem: https://leetcode.com/problems/perfect-squares/editorial/

Solution: https://programmercarl.com/0322.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

- Time complexity: O(n⋅sqrt(n))
- Space Complexity: O(n)

```python
# 第一种
class Solution:
    def numSquares(self, n: int) -> int:
        items = [i**2 for i in range(1 ,int(sqrt(n)) + 1 )]
        print(items)
        dp = [float('inf')] * (n + 1)
        # bottom case
        dp[0] = 0
        # 先遍历背包
        for j in range(n + 1):
            # 再遍历物品
            for item in items:
                if j >= item:
                    dp[j] = min(dp[j] , dp[j - item] + 1)
        return dp[n]

# 第二种
class Solution:
    def numSquares(self, n: int) -> int:
        items = [i**2 for i in range(1 ,int(sqrt(n)) + 1 )]
        print(items)
        dp = [float('inf')] * (n + 1)
        # bottom case
        dp[0] = 0
        # 先遍历物品
        for item in items:
            # 再遍历背包
            for j in range(item, n + 1):
                    dp[j] = min(dp[j] , dp[j - item] + 1)
        return dp[n]
```