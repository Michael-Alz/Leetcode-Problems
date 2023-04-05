# Day 35 | 860 Lemonade Change | 406 Queue Reconstruction by Height | 452 Minimum Number of Arrows to Burst Balloons

## 860 Lemonade Change

Problem: https://leetcode.com/problems/lemonade-change/description/

Solution: https://programmercarl.com/0860.%E6%9F%A0%E6%AA%AC%E6%B0%B4%E6%89%BE%E9%9B%B6.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

有如下三种情况：

- 情况一：账单是5，直接收下。
- 情况二：账单是10，消耗一个5，增加一个10
- 情况三：账单是20，优先消耗一个10和一个5，如果不够，再消耗三个5
- Time Complexity: O(N)
- Space Complexity: O(1)

```python
class Solution:
    def lemonadeChange(self, bills: List[int]) -> bool:
        five, ten = 0 , 0
        for bill in bills:
            if bill == 5:
                five += 1
            elif bill == 10:
                if five < 1:
                    return False
                else:
                    five -= 1
                    ten += 1
            elif bill == 20:
                if ten >= 1 and five >= 1:
                    five -= 1
                    ten -= 1
                elif five >= 3:
                    five -= 3
                else:
                    return False
        return True
```

## 406 Queue Reconstruction by Height

Problem: https://leetcode.com/problems/queue-reconstruction-by-height/description/

Solution: https://www.programmercarl.com/0406.%E6%A0%B9%E6%8D%AE%E8%BA%AB%E9%AB%98%E9%87%8D%E5%BB%BA%E9%98%9F%E5%88%97.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

- Time complexity : O(N^2) To sort people takes O(Nlog⁡N) time.
- Space complexity : O(N)

**局部最优：优先按身高高的people的k来插入。插入操作过后的people满足队列属性**

**全局最优：最后都做完插入操作，整个队列满足题目队列属性**

```python
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
    	# 先按照h维度的身高顺序从高到低排序。确定第一个维度
        # lambda返回的是一个元组：当-x[0](维度h）相同时，再根据x[1]（维度k）从小到大排序
        people.sort(key=lambda x: (-x[0], x[1]))
        que = []
				# 根据每个元素的第二个维度k，贪心算法，进行插入
        # people已经排序过了：同一高度时k值小的排前面。
        for p in people:
            # list.insert(position, elem)
            que.insert(p[1],p)
        return que
```

## 452 Minimum Number of Arrows to Burst Balloons

Problem: https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/

Solution: https://www.programmercarl.com/0452.%E7%94%A8%E6%9C%80%E5%B0%91%E6%95%B0%E9%87%8F%E7%9A%84%E7%AE%AD%E5%BC%95%E7%88%86%E6%B0%94%E7%90%83.html#%E6%80%9D%E8%B7%AF

- Time complexity : O(Nlog⁡N)
- Space complexity : O(N)

![452.用最少数量的箭引爆气球](https://code-thinking-1253855093.file.myqcloud.com/pics/20201123101929791.png)

```python
class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        if len(points) == 0:
            return 0
        # points 不为空至少需要一支箭
        res = 1
        # 根据气球直径的开始坐标从小到大排序
        points.sort(key=lambda x: x[0])
        for i in range(1 , len(points)):
            # 气球i和气球i-1不挨着，注意这里不是>=
            if points[i][0] > points[i - 1][1]:
                res += 1
           
            # 气球i和气球i-1挨着
            else:
                # 更新重叠气球最小右边界
                points[i][1] = min(points[i][1], points[i - 1][1])

        return res
```