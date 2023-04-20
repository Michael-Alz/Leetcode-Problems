# Day 51 | 309 Best Time to Buy and Sell Stock with Cooldown | 714 Best Time to Buy and Sell Stock with Transaction Fee

## 309 Best Time to Buy and Sell Stock with Cooldown

Problem:https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/

Solution:https://programmercarl.com/0309.%E6%9C%80%E4%BD%B3%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E6%97%B6%E6%9C%BA%E5%90%AB%E5%86%B7%E5%86%BB%E6%9C%9F.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/editorial/

- 时间复杂度：O(n)
- 空间复杂度：O(n)

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
# dp[i][j]，第i天状态为j，所剩的最多现金为dp[i][j]。
# 状态一：持有股票状态（今天买入股票，或者是之前就买入了股票然后没有操作，一直持有）
# 不持有股票状态，这里就有两种卖出股票状态
# 状态二：保持卖出股票的状态（两天前就卖出了股票，度过一天冷冻期。或者是前一天就是卖出股票状态，一直没操作）(冷冻期之后没有操作)
# 状态三：今天卖出股票
# 状态四：今天为冷冻期状态，但冷冻期状态不可持续，只有一天！
        n = len(prices)
        if n == 0:
            return 0
        dp = [4 * [0] for _ in range(n)]
        dp[0][0] = -prices[0]
        for i in range(1,n):
            # 前一天就持有；今天买入：前一天是保持卖出股票状态或者前一天是冷冻期
            dp[i][0] = max(dp[i - 1][0], max(dp[i - 1][1],dp[i - 1][3]) - prices[i])
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][3])
            dp[i][2] = dp[i - 1][0] + prices[i]
            dp[i][3] = dp[i - 1][2]
        return max(dp[n - 1][3], dp[n - 1][1], dp[n - 1][2])
```

## 714 Best Time to Buy and Sell Stock with Transaction Fee

Problem: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/

Solution: https://programmercarl.com/0714.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BA%E5%90%AB%E6%89%8B%E7%BB%AD%E8%B4%B9%EF%BC%88%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%EF%BC%89.html

卖股票的时候减去手续费

- 时间复杂度：O(n)
- 空间复杂度：O(n)

```python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        n = len(prices)
        if n == 0:
            return 0
        dp = [[0] * 2 for _ in range(n)]
        # dp[i][0] 表示第i天持有股票所得最多现金
        # dp[i][1] 表示第i天不持有股票所得最多现金
        dp[0][0] = -prices[0]
        for i in range(1, n):
            dp[i][0] = max(dp[i -1][0], dp[i - 1][1] - prices[i])
            dp[i][1] = max(dp[i -1][1], dp[i - 1][0] + prices[i] - fee)
        return dp[-1][1]
# 时间复杂度：O(n)
# 空间复杂度：O(n)
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        n = len(prices)
        hold, free = [0] * n, [0] * n
        
        # In order to hold a stock on day 0, we have no other choice but to buy it for prices[0].
        hold[0] = -prices[0]
        
        for i in range(1, n):
            hold[i] = max(hold[i - 1], free[i - 1] - prices[i])
            free[i] = max(free[i - 1], hold[i - 1] + prices[i] - fee)
        
        return free[-1]
```