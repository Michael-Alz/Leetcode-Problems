# Day 32 | 122 Best Time to Buy and Sell Stock II | 55,45 Jump Game & II

## 122 Best Time to Buy and Sell Stock II

Problem: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/

Solution: https://programmercarl.com/0122.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BAII.html#%E6%80%9D%E8%B7%AF

- 收集正利润的区间，就是股票买卖的区间，而我们只需要关注最终利润，不需要记录区间
- 局部最优：收集每天的正利润，全局最优：求得最大利润
- 时间复杂度：O(n)
- 空间复杂度：O(1)

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        max_profit = 0
        for i in range(1,len(prices)):
            diff = prices[i] - prices[i-1]
            if diff > 0:
                max_profit += diff
        return max_profit
```

## 55 Jump Game

Problem: https://leetcode.com/problems/jump-game/description/

Solution: https://programmercarl.com/0055.%E8%B7%B3%E8%B7%83%E6%B8%B8%E6%88%8F.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

贪心算法局部最优解：每次取最大跳跃步数（取最大覆盖范围），整体最优解：最后得到整体最大覆盖范围，看是否能到终点。

- Time complexity : O(n)
- Space complexity : O(1)

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        if len(nums) == 1:
            return True
        i = 0
        max_cover = 0
        # 要在覆盖范围内遍历
        while i <= max_cover:
            # 增加覆盖范围
            max_cover = max(max_cover, nums[i] + i)
            if max_cover >= len(nums) - 1:
                return True
            i += 1
        return False
```

## 45 Jump Game II

Problem: https://leetcode.com/problems/jump-game-ii/description/

Solution: https://programmercarl.com/0045.%E8%B7%B3%E8%B7%83%E6%B8%B8%E6%88%8FII.html#%E6%96%B9%E6%B3%95%E4%B8%80

Time complexity: O(n) Space complexity: O(1)

贪心的思路，局部最优：当前可移动距离尽可能多走，如果还没到终点，步数再加一。整体最优：一步尽可能多走，从而达到最小步数。

特殊情况需要考虑，当移动下标达到了当前覆盖的最远距离下标时

- 如果当前覆盖最远距离下标不是是集合终点，步数就加一，还需要继续走。
- 如果当前覆盖最远距离下标就是是集合终点，步数不用加一，因为不能再往后走了。

```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return 0
        cur_cover, next_cover = 0, 0 #当前和下一步覆盖最远距离下标
        res = 0 # 记录走的步数
        for i in range(len(nums)):
            # 更新下一步覆盖最远距离下标
            next_cover = max(next_cover, nums[i] + i)
            # 如果走到当前覆盖最远距离下标
            if i == cur_cover:
                res += 1 
                # 更新当前覆盖最远距离下标
                cur_cover = next_cover 
                # 当前覆盖最远距到达集合终点，不用做res += 1操作了，直接结束
                if next_cover >= len(nums) - 1:
                    break
        return res
```