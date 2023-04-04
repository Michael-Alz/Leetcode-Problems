# Day 34 | 1005 Maximize Sum Of Array After K Negations | 134 Gas Station | 135 Candy

## 1005 Maximize Sum Of Array After K Negations

Problem: https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/

Solution: https://programmercarl.com/1005.K%E6%AC%A1%E5%8F%96%E5%8F%8D%E5%90%8E%E6%9C%80%E5%A4%A7%E5%8C%96%E7%9A%84%E6%95%B0%E7%BB%84%E5%92%8C.html

局部最优：让绝对值大的负数变为正数，当前数值达到最大，整体最优：整个数组和达到最大。

那么如果将负数都转变为正数了，K依然大于0，此时的问题是一个有序正整数序列，如何转变K次正负，让 数组和 达到最大。

那么又是一个贪心：局部最优：只找数值最小的正整数进行反转，当前数值和可以达到最大（例如正整数数组{5, 3, 1}，反转1 得到-1 比 反转5得到的-5 大多了），全局最优：整个 数组和 达到最大。

Time: O(n)

Space: O(1)

```python
class Solution:
    def largestSumAfterKNegations(self, nums: List[int], k: int) -> int:
				# 通过绝对值从大到小排序
        nums.sort(reverse = True, key = abs)
				# 把负数从绝对值最大到最小变成正数
        for i in range(len(nums)):
            if nums[i] < 0:
                nums[i] = -nums[i]
                k -= 1
                if k == 0:
                    break
				# 如果这时k依旧大于0，就把最小的正数变换 -1^k 
        if k > 0:
            nums[i] = nums[i] * (-1)**k
        return sum(nums)
```

## 134 Gas Station

Problem: https://leetcode.com/problems/gas-station/description/ 

Solution: https://programmercarl.com/0134.%E5%8A%A0%E6%B2%B9%E7%AB%99.html#%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95-%E6%96%B9%E6%B3%95%E4%BA%8C

- Time complexity : O(N)
- Space complexity : O(1)

如果总油量减去总消耗大于等于零那么一定可以跑完一圈，说明 各个站点的加油站 剩油量rest[i]相加一定是大于等于零的。

每个加油站的剩余量rest[i]为gas[i] - cost[i]。

i从0开始累加rest[i]，和记为curSum，一旦curSum小于零，说明[0, i]区间都不能作为起始位置，因为这个区间选择任何一个位置作为起点，到i这里都会断油，那么起始位置从i+1算起，再从0计算curSum。

```python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        cursum = 0
        totalsum = 0
        index = 0
        for i in range(len(gas)):
            cursum += gas[i] - cost[i]
            totalsum += gas[i] - cost[i]
				# 当前累加rest[i]和 curSum一旦小于0
            if cursum < 0:
				# 起始位置更新为i+1
                index = i + 1
				# curSum从0开始
                cursum = 0
        # 说明怎么走都不可能跑一圈了
        if totalsum < 0 :
            return -1
        
        return index
```

## 135 Candy

Problem: https://leetcode.com/problems/candy/description/

Solution: https://programmercarl.com/0135.%E5%88%86%E5%8F%91%E7%B3%96%E6%9E%9C.html#%E6%80%9D%E8%B7%AF

- 一次是从左到右遍历，只比较右边孩子评分比左边大的情况。
- 一次是从右到左遍历，只比较左边孩子评分比右边大的情况。
- Time complexity : O(n)
- Space complexity : O(n)

```python
class Solution:
    def candy(self, ratings: List[int]) -> int:
        candynum = [1 for _ in range(len(ratings))]
				 # 分两个阶段
         # 1、起点下标1 从左往右，只要 右边 比 左边 大，右边的糖果=左边 + 1
         # 2、起点下标 len(ratings) - 2 从右往左， 
				 # 只要左边 比 右边 大，此时 左边的糖果应该 取本身的糖果数（符合比它左边大） 
				 # 和 右边糖果数 + 1 二者的最大值，这样才符合 它比它左边的大，也比它右边大

        for i in range(1,len(ratings)):
            if ratings[i] > ratings[i - 1]:
                candynum[i] =candynum[i - 1] + 1
        
        for i in range(len(ratings) - 2 , -1 , -1):
            if ratings[i] > ratings[i + 1]:
                candynum[i] = max(candynum[i] , candynum[i + 1] + 1)

        return sum(candynum)
```