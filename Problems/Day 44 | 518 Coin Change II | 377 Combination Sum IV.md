# Day 44 | 518 Coin Change II | 377 Combination Sum IV

## 完全背包

https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85.html

~~~python
# 先遍历物品，再遍历背包
def test_complete_pack1():
    weight = [1, 3, 4]
    value = [15, 20, 30]
    bag_weight = 4

    dp = [0]*(bag_weight + 1)

    for i in range(len(weight)):
        for j in range(weight[i], bag_weight + 1):
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i])
    
    print(dp[bag_weight])

# 先遍历背包，再遍历物品
def test_complete_pack2():
    weight = [1, 3, 4]
    value = [15, 20, 30]
    bag_weight = 4

    dp = [0]*(bag_weight + 1)

    for j in range(bag_weight + 1):
        for i in range(len(weight)):
            if j >= weight[i]: dp[j] = max(dp[j], dp[j - weight[i]] + value[i])
    
    print(dp[bag_weight])


if __name__ == '__main__':
    test_complete_pack1()
    test_complete_pack2()
~~~

## 518 Coin Change II

Problem: https://leetcode.com/problems/coin-change-ii/description/

Solution: https://leetcode.com/problems/coin-change-ii/description/

dp[j]：凑成总金额j的货币组合数为dp[j]

dp递推公式和494一样(求排列和组合可以都用这个公式）

**如果求组合数就是外层for循环遍历物品，内层for遍历背包**。

**如果求排列数就是外层for遍历背包，内层for循环遍历物品**。

先遍历物品后遍历背包是这样，比如，外层循环固定coins【1】，在内层循环遍历背包时，随着背包不断增加，coins【1】可以重复被添加进来，而由于外层循环固定了，因此coins【2】只能在下一次外层循环添加进不同大小的背包中，这么看的话，coins【i+1】只能在coins【i】之后了；如果先遍历背包后遍历物品，那么外层循环先固定背包大小j，然后在大小为j的背包中循环遍历添加物品，然后在下次外层循环背包大小变为j+1，此时仍要执行内层循环遍历添加物品，也就会出现在上一轮外层循环中添加coins【2】的基础上还能再添加coins【1】的情况，那么就有了coins【1】在coins【2】之后的情况了。

- Time complexity: O(N×amount)
- Space complexity: O(amount)

```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        dp = [0]*(amount + 1)
        dp[0] = 1
        # 遍历物品
        for i in range(len(coins)):
            # 遍历背包
            for j in range(coins[i], amount + 1):
                dp[j] += dp[j - coins[i]]
        return dp[amount]
```

## 377 Combination Sum IV

Problem: https://leetcode.com/problems/combination-sum-iv/description/

Solution: https://programmercarl.com/0377.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C%E2%85%A3.html#%E6%80%9D%E8%B7%AF

先遍历背包，再遍历物品，可以得到排列

Time Complexity: O(T⋅N) Space Complexity: O(T)

```python
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        dp = [0] * (target + 1)
        dp[0] = 1
        for i in range(0 , len(dp)):
            for j in range(0 , len(nums)):
                if i >= nums[j]:
                    dp[i] += dp[i - nums[j]]
        return dp[target]
```