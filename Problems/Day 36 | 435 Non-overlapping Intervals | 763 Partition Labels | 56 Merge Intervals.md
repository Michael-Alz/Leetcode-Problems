# Day 36 | 435 Non-overlapping Intervals | 763 Partition Labels | 56 Merge Intervals

## 435 Non-overlapping Intervals

Problem: https://leetcode.com/problems/non-overlapping-intervals/description/

Solution: https://www.programmercarl.com/0435.%E6%97%A0%E9%87%8D%E5%8F%A0%E5%8C%BA%E9%97%B4.html#%E6%80%9D%E8%B7%AF

本质上是计算重叠的区间数量，相等的边界不算重叠

- Time complexity : O(nlog⁡(n))
- Space complexity : O(1)

```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        count = 0
        if len(intervals) == 1:
            return count
        intervals.sort(key = lambda x : x[0])
        for i in range(1, len(intervals)):
            if intervals[i][0] < intervals[i - 1][1]:
                count += 1
                intervals[i][1] = min(intervals[i][1], intervals[i - 1][1])
        return count
```

## 763 Partition Labels

Problem: https://leetcode.com/problems/partition-labels/description/

Solution: https://www.programmercarl.com/0763.%E5%88%92%E5%88%86%E5%AD%97%E6%AF%8D%E5%8C%BA%E9%97%B4.html#%E6%80%9D%E8%B7%AF

- Time Complexity: O(N)
- Space Complexity: O(1)

可以分为如下两步：

- 统计每一个字符最后出现的位置
- 从头遍历字符，并更新字符的最远出现下标，如果找到字符最远出现位置下标和当前下标相等了，则找到了分割点

![763.划分字母区间](https://code-thinking-1253855093.file.myqcloud.com/pics/20201222191924417.png)

```python
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        # i为字符，hash[i]为字符出现的最后位置 
        hashtable = 26 * [0]
        # 统计每个字母在s里最远出现下标
        for i in range(len(s)):
            hashtable[ord(s[i]) - ord('a')] = i
        res = []
        left = 0
        right = 0
        for i in range(len(s)):
            right = max(right, hashtable[ord(s[i]) - ord('a')])
            # 找到字符出现的最远边界
            if i == right:
            # 计算区间字母数量
                res.append(right - left + 1)
            # 更新下一区间左区间
                left = right + 1
        return res
```

## 56 Merge Intervals

Problem: https://leetcode.com/problems/merge-intervals/description/

Solution: https://www.programmercarl.com/0056.%E5%90%88%E5%B9%B6%E5%8C%BA%E9%97%B4.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

- Time complexity : O(nlog⁡n)
- Space complexity : O(log⁡N) (or O(n))

按照左边界从小到大排序之后，如果 `intervals[i][0] <= intervals[i - 1][1]` 即intervals[i]的左边界 <= intervals[i - 1]的右边界，则一定有重叠。（本题相邻区间也算重贴，所以是<=）

用合并区间后左边界和右边界，作为一个新的区间，加入到result数组里就可以了。如果没有合并就把原区间加入到result数组。

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        if len(intervals) == 1:
            return intervals
        intervals.sort(key = lambda x: x[0])
        res = []
        # 第一个区间就可以放进结果集里，后面如果重叠，在result上直接合并
        res.append(intervals[0])
        for i in range(1 , len(intervals)):
            last = res[-1]
            # 发现重叠区间
            if intervals[i][0] <= last[1]:
                # 合并区间，只更新右边界就好，因为result.back()的左边界一定是最小值，因为我们按照左边界排序的
                res[-1] = [last[0],max(intervals[i][1], last[1])]
            else: # 区间不重叠 
                res.append(intervals[i])
        return res
```