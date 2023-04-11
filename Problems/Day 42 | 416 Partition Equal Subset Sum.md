# Day 42 | 416 Partition Equal Subset Sum

## 0-1 背包基础理论

二维dp数组：https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html#%E4%BA%8C%E7%BB%B4dp%E6%95%B0%E7%BB%8401%E8%83%8C%E5%8C%85

一维dp数组：https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-2.html#%E4%B8%80%E7%BB%B4dp%E6%95%B0%E7%BB%84-%E6%BB%9A%E5%8A%A8%E6%95%B0%E7%BB%84

## 416 Partition Equal Subset Sum

Problem: https://leetcode.com/problems/partition-equal-subset-sum/description/

Solution: https://programmercarl.com/0416.%E5%88%86%E5%89%B2%E7%AD%89%E5%92%8C%E5%AD%90%E9%9B%86.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

确定01背包问题：

- 背包的体积为sum / 2
- 背包要放入的商品（集合里的元素）重量为 元素的数值，价值也为元素的数值
- 背包如果正好装满，说明找到了总和为 sum / 2 的子集。
- 背包中每一个元素是不可重复放入。

动规五部曲分析如下：

1.确定dp数组以及下标的含义

01背包中，dp[j] 表示： 容量为j的背包，所背的物品价值最大可以为dp[j]。

本题中每一个元素的数值既是重量，也是价值。

**套到本题，dp[j]表示 背包总容量（所能装的总重量）是j，放进物品后，背的最大重量为dp[j]**。

那么如果背包容量为target， dp[target]就是装满 背包之后的重量，所以 当 dp[target] == target 的时候，背包就装满了。

有录友可能想，那还有装不满的时候？

拿输入数组 [1, 5, 11, 5]，举例， dp[7] 只能等于 6，因为 只能放进 1 和 5。

而dp[6] 就可以等于6了，放进1 和 5，那么dp[6] == 6，说明背包装满了。

2.确定递推公式

01背包的递推公式为：dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);

本题，相当于背包里放入数值，那么物品i的重量是nums[i]，其价值也是nums[i]。

所以递推公式：dp[j] = max(dp[j], dp[j - nums[i]] + nums[i]);

3.dp数组如何初始化

在01背包，一维dp如何初始化，已经讲过，

从dp[j]的定义来看，首先dp[0]一定是0。

如果题目给的价值都是正整数那么非0下标都初始化为0就可以了，如果题目给的价值有负数，那么非0下标就要初始化为负无穷。

**这样才能让dp数组在递推的过程中取得最大的价值，而不是被初始值覆盖了**。

本题题目中 只包含正整数的非空数组，所以非0下标的元素初始化为0就可以了。

4.确定遍历顺序

如果使用一维dp数组，物品遍历的for循环放在外层，遍历背包的for循环放在内层，且内层for循环倒序遍历.

5.举例推导dp数组

dp[j]的数值一定是小于等于j的。

**如果dp[j] == j 说明，集合中的子集总和正好可以凑成总和j，理解这一点很重要。**

一维数组：

- Time Complexity : O(m⋅n)
- Space Complexity: O(m)

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        target = sum(nums)
        if target % 2 == 1:
            return False
        target = target // 2
        dp = [0] * (target + 1)
				# 遍历物品
        for i in range(len(nums)):
						# 遍历背包，倒叙遍历
            for j in range(target, nums[i] - 1, -1):
                dp[j] = max(dp[j], dp[j - nums[i]] + nums[i])
        if dp[target] == target:
            return True
```

二维数组：

- Time Complexity : O(m⋅n)
- Space Complexity: O(m⋅n)

```python
# 二维度数组解法
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        target = sum(nums)
        nums = sorted(nums)

        # 做最初的判断
        if target % 2 != 0:
            return False

        # 找到 target value 可以认为这个是背包的体积
        target = target // 2

        row = len(nums)
        col = target + 1

        # 创建二维状态数组，行：物品索引，列：容量（包括 0）
        # dp[i][j]表示从数组的 [0, i] 这个子区间内挑选一些正整数
        # 每个数只能用一次，使得这些数的和恰好等于 j。
        dp = [[0 for _ in range(col)] for _ in range(row)] 

        # 初始 dp value, 第一列
        for i in range(row):
            dp[i][0] = 0
        # 第一行，如果第一个数字小于容量，则把数字的价值装入
        for j in range(1, target):
            if nums[0] <= j:
                dp[0][j] = nums[0]

        # 遍历 先遍历物品再遍历背包
        for i in range(1, row):

            cur_weight = nums[i]
            cur_value = nums[i]

            for j in range(1, col):
                if cur_weight > j:
                    dp[i][j] = dp[i - 1][j]
                else:
                    dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - cur_weight] + cur_value)
            
        # 输出结果
        return dp[-1][col - 1] == target
```