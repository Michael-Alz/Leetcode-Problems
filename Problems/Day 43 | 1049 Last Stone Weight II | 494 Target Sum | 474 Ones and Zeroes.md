# Day 43 | 1049 Last Stone Weight II | 494 Target Sum | 474 Ones and Zeroes

## 1049 Last Stone Weight II

Problem: https://leetcode.com/problems/last-stone-weight-ii/description/

Solution: https://programmercarl.com/1049.%E6%9C%80%E5%90%8E%E4%B8%80%E5%9D%97%E7%9F%B3%E5%A4%B4%E7%9A%84%E9%87%8D%E9%87%8FII.html#%E6%80%9D%E8%B7%AF

思路为把尽量把石头分为重量相同的两部分

最后dp[target]里是容量为target的背包所能背的最大重量。

那么分成两堆石头，一堆石头的总重量是dp[target]，另一堆就是sum - dp[target]。

**在计算target的时候，target = sum / / 2 因为是向下取整，所以sum - dp[target] 一定是大于等于dp[target]的**。

那么相撞之后剩下的最小石头重量就是 (sum - dp[target]) - dp[target]。

- 时间复杂度：O(m × n) , m是石头总重量（准确的说是总重量的一半），n为石头块数
- 空间复杂度：O(m)

```python
class Solution:
    def lastStoneWeightII(self, stones) -> int:
        target = sum(stones) // 2
        dp = [0] * (target + 1)
        for i in range(len(stones)):
            for j in range(target, stones[i] - 1, -1):
                dp[j] = max(dp[j], dp[j - stones[i]] + stones[i])
				# 计算剩下的最小石头重量
        return sum(stones) - dp[-1]  - dp[-1]
```

## 494 Target Sum

Problem: https://leetcode.com/problems/target-sum/description/

Solution: https://www.programmercarl.com/0494.%E7%9B%AE%E6%A0%87%E5%92%8C.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

Let `P` be the positive subset and `N` be the negative subsetFor example:Given `nums = [1, 2, 3, 4, 5]` and `target = 3` then one possible solution is `+1-2+3-4+5 = 3`Here positive subset is `P = [1, 3, 5]` and negative subset is `N = [2, 4]`

Then let's see how this can be converted to a subset sum problem:

```
              `sum(P) - sum(N) = target
```

sum(P) + sum(N) + sum(P) - sum(N) = target + sum(P) + sum(N) 2 * sum(P) = target + sum(nums)`

1. 确定dp数组以及下标的含义

   dp[j] 表示：填满j（包括j）这么大容积的包，有dp[j]种方法

   其实也可以使用二维dp数组来求解本题，dp[i][j]：使用 下标为[0, i]的nums[i]能够凑满j（包括j）这么大容量的包，有dp[i][j]种方法。

2. 确定递推公式

   有哪些来源可以推出dp[j]呢？

   只要搞到nums[i]），凑成dp[j]就有dp[j - nums[i]] 种方法。

   例如：dp[j]，j 为5，

   - 已经有一个1（nums[i]） 的话，有 dp[4]种方法 凑成 容量为5的背包。
   - 已经有一个2（nums[i]） 的话，有 dp[3]种方法 凑成 容量为5的背包。
   - 已经有一个3（nums[i]） 的话，有 dp[2]中方法 凑成 容量为5的背包
   - 已经有一个4（nums[i]） 的话，有 dp[1]中方法 凑成 容量为5的背包
   - 已经有一个5 （nums[i]）的话，有 dp[0]中方法 凑成 容量为5的背包

   那么凑整dp[5]有多少方法呢，也就是把 所有的 dp[j - nums[i]] 累加起来。

   所以求组合类问题的公式，都是类似这种：

   `dp[j] += dp[j - nums[i]]`

3. dp数组如何初始化

   从递推公式可以看出，在初始化的时候dp[0] 一定要初始化为1，因为dp[0]是在公式中一切递推结果的起源，如果dp[0]是0的话，递推结果将都是0。

   这里有录友可能认为从dp数组定义来说 dp[0] 应该是0，也有录友认为dp[0]应该是1。

   其实不要硬去解释它的含义，咱就把 dp[0]的情况带入本题看看应该等于多少。

   如果数组[0] ，target = 0，那么 bagSize = (target + sum) / 2 = 0。 dp[0]也应该是1， 也就是说给数组里的元素 0 前面无论放加法还是减法，都是 1 种方法。

   所以本题我们应该初始化 dp[0] 为 1。

   可能有同学想了，那 如果是 数组[0,0,0,0,0] target = 0 呢。

   其实 此时最终的dp[0] = 32，也就是这五个零 子集的所有组合情况，但此dp[0]非彼dp[0]，dp[0]能算出32，其基础是因为dp[0] = 1 累加起来的。

   dp[j]其他下标对应的数值也应该初始化为0，从递推公式也可以看出，dp[j]要保证是0的初始值，才能正确的由dp[j - nums[i]]推导出来。

   - 时间复杂度：O(n × m)，n为正数个数，m为背包容量
   - 空间复杂度：O(m)，m为背包容量

   ```python
   class Solution:
       def findTargetSumWays(self, nums: List[int], target: int) -> int:
           # left is the set of positive nums, right is set of negative nums
           # left + right = total
           # left - right = target
           # left = (total + target) / 2
           total = sum(nums)
           if (total + target) % 2 == 1 or abs(target) > total:
               return 0
           bagsize = int((total + target) / 2)
           dp =(bagsize + 1) * [0]
           dp[0] = 1
           for i in range(len(nums)):
               for j in range(bagsize , nums[i] - 1 ,-1):
                   dp[j] += dp[j - nums[i]]
           return dp[-1]
   ```

## 474 Ones and Zeroes

1. 确定dp数组（dp table）以及下标的含义

   dp[i][j]：最多有i个0和j个1的strs的最大子集的大小为dp[i][j]

2. 确定递推公式

   dp[i][j] 可以由前一个strs里的字符串推导出来，strs里的字符串有zeroNum个0，oneNum个1。

   dp[i][j] 就可以是 dp[i - zeroNum][j - oneNum] + 1。

   然后我们在遍历的过程中，取dp[i][j]的最大值。

   所以递推公式：dp[i][j] = max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);

   此时大家可以回想一下01背包的递推公式：dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);

   对比一下就会发现，字符串的zeroNum和oneNum相当于物品的重量（weight[i]），字符串本身的个数相当于物品的价值（value[i]）。

   **这就是一个典型的01背包！** 只不过物品的重量有了两个维度而已。

3. 因为物品价值不会是负数，初始为0，保证递推的时候dp[i][j]不会被初始值覆盖。

- Time complexity : O(len(strs)*m∗n)
- Space complexity : O(m∗n)

```python
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        # 二维dp数组
        # dp[i][j]表示i个0和j个1时的最大子集
        dp = [[0 for i in range(n + 1)] for i in range(m + 1)]
        # 遍历物品
        for s in strs:
            zeros = s.count('0')
            ones = s.count('1')
            # 遍历背包容量且从后向前遍历！
            for i in range(m , zeros - 1, -1 ):
                for j in range(n , ones - 1 , -1):
                    dp[i][j] = max(dp[i][j], dp[i - zeros][j - ones] + 1)
        return dp[m][n]
```