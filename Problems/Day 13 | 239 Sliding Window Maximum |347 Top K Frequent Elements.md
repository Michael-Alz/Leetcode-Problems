# Day 13 | 239 Sliding Window Maximum |347 Top K Frequent Elements

## 239 Sliding Window Maximum

Problem: https://leetcode.com/problems/sliding-window-maximum/description/

Solution: https://www.programmercarl.com/0239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC.html

用单调队列实现排序

用单调队列的时候，pop，和push操作要保持如下规则：

1. pop(value)：如果窗口移除的元素value等于单调队列的出口元素，那么队列弹出元素，否则不用任何操作
2. push(value)：如果push的元素value大于入口元素的数值，那么就将队列入口的元素弹出，直到push元素的数值小于等于队列入口元素的数值为止

保持如上规则，每次窗口移动的时候，只要问que.front()就可以返回当前窗口的最大值。

再来看一下时间复杂度，使用单调队列的时间复杂度是 O(n)。

单调队列的实现，nums 中的每个元素最多也就被 push_back 和 pop_back 各一次，没有任何多余操作，所以整体的复杂度还是 O(n)。

空间复杂度因为我们定义一个辅助队列，所以是O(k)。

~~~python
from collections import deque
class my_queue:
#自定义一个单调队列,需要用deque实现，否则会超时
    def __init__(self):
        self.queue = deque()
    #定义弹出元素，如果出去的元素与队头元素相等，那么弹出队头元素。否则不进行操作。
    def pop (self, x):
        if self.queue and self.queue[0] == x:
            self.queue.popleft()
    #如果进来的元素比队尾元素大，那么一直弹出队尾元素，直到不满足条件时，放入新元素。
    #当然如果要进来的元素比队尾元素小，那么直接放入就可以。
    def push(self,x):
        while self.queue and x >  self.queue[-1]:
            self.queue.pop()
        self.queue.append(x)
    #找出最大值,只需要return队头的元素即可
    def max_val(self):
        return self.queue[0]

            
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        q = my_queue()
        result = []
        n = len(nums)
        #先把前k个元素放入队列
        for i in range(k):
            q.push(nums[i])
        result.append(q.max_val())
        #把剩下的元素pop，再push，再max_val：
        for i in range(k , n):
            #把滑动窗口的头部pop掉，这里是滑动窗口当前应该pop掉的元素
            #比如第二次是1必须出去，第三次是3必须出去
            q.pop(nums[i - k]) 
            q.push(nums[i]) #把后面的push到队列里
            result.append(q.max_val())
        return result
~~~

## 347 Top K Frequent Elements

用小顶堆实现：

https://docs.python.org/3/library/heapq.html

- Time complexity : O(Nlog⁡k)
- Space complexity : O(N+k)

~~~python
import heapq
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        #统计出现的元素次数
        my_map = {}
        for i in nums:
            #dictionary.get(keyname, value) 
            #value: A value to return if the specified key does not exist.
            my_map[i] = my_map.get(i,0) + 1
        #对于频率排序，并找出最多的k个元素
        #用heapq实现（小顶堆）
        heap = []
        #把字典遍历到heap里，用value（次数）作为key来排序
        for key, value in my_map.items():
            heapq.heappush(heap,(value,key))
        #留k个最大次数，只需要pop掉len(heap) - k个元素，小顶堆是从最小的开始pop
        for i in range(len(heap) - k):
            heapq.heappop(heap)
        res = []
        for i,j in heap:
            res.append(j)
        #倒序输出
        return res[::-1]
~~~

## 栈与队列总结

总结：https://www.programmercarl.com/%E6%A0%88%E4%B8%8E%E9%98%9F%E5%88%97%E6%80%BB%E7%BB%93.html

Python 小顶堆实现: https://docs.python.org/3/library/heapq.html