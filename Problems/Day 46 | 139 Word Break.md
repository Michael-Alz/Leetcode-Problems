# Day 46 | 139 Word Break

## 139 Word Break

Problem:https://leetcode.com/problems/word-break/

Solution:https://programmercarl.com/0139.%E5%8D%95%E8%AF%8D%E6%8B%86%E5%88%86.html#%E6%80%9D%E8%B7%AF

- Time complexity : O(n^3)
- Space complexity : O(n)
- 确定dp数组以及下标的含义

**dp[i] : 字符串长度为i的话，dp[i]为true，表示可以拆分为一个或多个在字典中出现的单词**。

- 确定递推公式

如果确定dp[j] 是true，且 [j, i] 这个区间的子串出现在字典里，那么dp[i]一定是true。（j < i ）。

所以递推公式是 if([j, i] 这个区间的子串出现在字典里 && dp[j]是true) 那么 dp[i] = true。

- dp数组如何初始化

从递推公式中可以看出，dp[i] 的状态依靠 dp[j]是否为true，那么dp[0]就是递推的根基，dp[0]一定要为true，否则递推下去后面都都是false了。

那么dp[0]有没有意义呢？

dp[0]表示如果字符串为空的话，说明出现在字典里。

但题目中说了“给定一个非空字符串 s” 所以测试数据中不会出现i为0的情况，那么dp[0]初始为true完全就是为了推导公式。

下标非0的dp[i]初始化为false，只要没有被覆盖说明都是不可拆分为一个或多个在字典中出现的单词。

- 确定遍历顺序

题目中说是拆分为一个或多个在字典中出现的单词，所以这是完全背包。

还要讨论两层for循环的前后顺序。

**如果求组合数就是外层for循环遍历物品，内层for遍历背包**。

**如果求排列数就是外层for遍历背包，内层for循环遍历物品**。

```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        # dp[i] : 字符串长度为i的话，dp[i]为true，表示可以拆分为一个或多个在字典中出现的单词
        dp = [False] * (len(s) + 1)
        dp[0] = True
        # 遍历背包（排列问题先遍历背包）
        for i in range(len(s) + 1):
            for j in range(i):
                # 拆分成物品，遍历物品
                word = s[j: i]
                if word in wordDict and dp[j]:
                    dp[i] = True
                    break
        return dp[len(s)]
```

## 多重背包

- 时间复杂度：O(m × n × k)，m：物品种类个数，n背包容量，k单类物品数量

~~~python
def test_multi_pack1():
    '''版本一：改变物品数量为01背包格式'''
    weight = [1, 3, 4]
    value = [15, 20, 30]
    nums = [2, 3, 2]
    bag_weight = 10
    for i in range(len(nums)):
        # 将物品展开数量为1
        while nums[i] > 1:
            weight.append(weight[i])
            value.append(value[i])
            nums[i] -= 1
    
    dp = [0]*(bag_weight + 1)
    # 遍历物品
    for i in range(len(weight)):
        # 遍历背包
        for j in range(bag_weight, weight[i] - 1, -1):
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i])
    
    print(" ".join(map(str, dp)))

def test_multi_pack2():
    '''版本：改变遍历个数'''
    weight = [1, 3, 4]
    value = [15, 20, 30]
    nums = [2, 3, 2]
    bag_weight = 10

    dp = [0]*(bag_weight + 1)
    for i in range(len(weight)):
        for j in range(bag_weight, weight[i] - 1, -1):
            # 以上是01背包，加上遍历个数
            for k in range(1, nums[i] + 1):
                if j - k*weight[i] >= 0:
                    dp[j] = max(dp[j], dp[j - k*weight[i]] + k*value[i])

    print(" ".join(map(str, dp)))


if __name__ == '__main__':
    test_multi_pack1()
    test_multi_pack2()
~~~

## 背包问题总结

https://programmercarl.com/%E8%83%8C%E5%8C%85%E6%80%BB%E7%BB%93%E7%AF%87.html#%E8%83%8C%E5%8C%85%E9%80%92%E6%8E%A8%E5%85%AC%E5%BC%8F