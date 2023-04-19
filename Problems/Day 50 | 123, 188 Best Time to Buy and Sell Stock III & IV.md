# Day 50 | 123, 188 Best Time to Buy and Sell Stock III & IV

## 123 Best Time to Buy and Sell Stock III

Problem: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/description/

Solution: https://programmercarl.com/0123.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BAIII.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

- 时间复杂度：O(n)
- 空间复杂度：O(n × 4)

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # 第一次持有股票: dp[i][0]
        # 第一次不持有股票: dp[i][1]
        # 第二次持有股票: dp[i][2]
        # 第二次不持有股票: dp[i][3]
        # dp[i][j]中 i表示第i天，j为 [0 - 3] 四个状态，dp[i][j]表示第i天状态j所剩最大现金。
        dp = [4 * [0] for _ in range(len(prices))]
        dp[0][0] = -prices[0]
        dp[0][2] = -prices[0]
        for i in range(1 , len(prices)):
            dp[i][0] = max(0 - prices[i], dp[i - 1][0])
            dp[i][1] = max(dp[i - 1][0] + prices[i], dp[i - 1][1])
            dp[i][2] = max(dp[i - 1][1] - prices[i], dp[i - 1][2])
            dp[i][3] = max(dp[i - 1][2] + prices[i], dp[i - 1][3])
        return dp[-1][3]
```

- 时间复杂度：O(n)
- 空间复杂度：O(1)

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if len(prices) == 0:
            return 0
        dp = [0] * 5 
        dp[1] = -prices[0]
        dp[3] = -prices[0]
        for i in range(1, len(prices)):
            dp[1] = max(dp[1], dp[0] - prices[i])
            dp[2] = max(dp[2], dp[1] + prices[i])
            dp[3] = max(dp[3], dp[2] - prices[i])
            dp[4] = max(dp[4], dp[3] + prices[i])
        return dp[4]
```

## 188 Best Time to Buy and Sell Stock IV

Problem:https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/description/

Solution:https://programmercarl.com/0188.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BAIV.html

可以买卖k次，所以创建 2k + 1 dp数组

思路: [**Best Time to Buy and Sell Stock III**](https://www.notion.so/Best-Time-to-Buy-and-Sell-Stock-III-c7910fbf740a40bd98e9071614fea5d1)

- Time Complexity: O(nk)
- Space Complexity: O(nk)

```python
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        if len(prices) == 0:
            return 0
        dp = [[0] * (2 * k + 1 ) for _ in range(len(prices))]
        for i in range(1, 2 * k + 1 , 2):
            dp[0][i] = -prices[0]
        for i in range(1, len(prices)):
            dp[i][0] = dp[i-1][0]           
            for j in range(1 , 2 * k + 1):
                dp[i][j] = max(dp[i - 1][j], dp[i -1][j -1] + (-1) ** j * prices[i])
        return dp[-1][2*k]
```

Time complexity: O(n*k)

Space complexity: O(k)

```python
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        if len(prices) == 0: return 0
        dp = [0] * (2*k + 1)
        for i in range(1,2*k,2):
            dp[i] = -prices[0]
        for i in range(1,len(prices)):
            for j in range(1,2*k + 1):
                if j % 2:
                    dp[j] = max(dp[j],dp[j-1]-prices[i])
                else:
                    dp[j] = max(dp[j],dp[j-1]+prices[i])
        return dp[2*k]
```