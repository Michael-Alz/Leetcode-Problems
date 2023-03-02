# Day 02 | 977 Squares of a Sorted Array| 209 Minimum Size Subarray Sum | 59 Spiral Matrix II

## 977 Squares of a Sorted Array

Problem: https://leetcode.com/problems/squares-of-a-sorted-array/description/

Solution: https://programmercarl.com/0977.%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E5%B9%B3%E6%96%B9.html#%E6%9A%B4%E5%8A%9B%E6%8E%92%E5%BA%8F

暴力解法：先平方，再用list.sort: O(n+nlogn)

~~~python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        for i in range (len(nums)):
            nums[i] *= nums[i]
        nums.sort()
        return nums
~~~

双指针法： O（N）

~~~python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
			#正数的相对位置是不变的， 需要调整的是负数平方后的相对位置
        n = len(nums)
        i,j,k = 0,n - 1,n - 1
        ans = [-1] * n
			#这个while的条件，n的指针能遍历整个ans
        while i <= j:
            lm = nums[i] ** 2
            rm = nums[j] ** 2
            if lm > rm:
                ans[k] = lm
                i += 1
            else:
                ans[k] = rm
                j -= 1
            k -= 1
        return ans
~~~

![img](https://code-thinking.cdn.bcebos.com/gifs/977.%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E5%B9%B3%E6%96%B9.gif)

## 209 Minimum Size Subarray Sum

Problem: https://leetcode.com/problems/minimum-size-subarray-sum/

Solution: https://programmercarl.com/0209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.html

暴力解法：O(N^2)  需要把遍历每个subarray

1. Check for our edge cases and possible O(1) solutions to the problem. (Reusable for Sliding Window Approach)

2. Define our variables that we will use to count the smallest window

3. Our Brute force that uses double for loops to generate the sum of all subarrays

4. return minimum if valid else return 0 

   ~~~python
   class Solution:
       def minSubArrayLen(self, target: int, nums: List[int]) -> int:
           if nums == None or len(nums) == 0:
               return 0
           if nums[0] >= target:
               return 1
   
           minimum = float("inf")
           for i in range(len(nums)):
               subarray_sum = 0
               for j in range(i,len(nums)):
                   subarray_sum += nums[j]
                   if j - i < minimum and subarray_sum == target:
                       minimum = j - i + 1
                   
           if minimum == float("inf"):
               return 0
           else:
               return minimum
   ~~~

   滑动窗口(双指针):

   窗口就是 满足其和 ≥ s 的长度最小的 连续 子数组。

   窗口的起始位置如何移动：如果当前窗口的值大于s了，窗口就要向前移动了（也就是该缩小了）。

   窗口的结束位置如何移动：窗口的结束位置就是遍历数组的指针，也就是for循环里的索引。

   解题的关键在于 窗口的起始位置如何移动，如图所示：

   ![209.长度最小的子数组](https://code-thinking.cdn.bcebos.com/gifs/209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.gif)

   ~~~python
   class Solution:
       def minSubArrayLen(self, target: int, nums: List[int]) -> int:
           i = 0
           j = 0
           res = float('inf')
           s = 0
           for j in range(len(nums)):
               #收集subarray的sum的值
               s = s + nums[j]
               #寻找目标subarray，并逐步缩小subarray(步长为1)，直到不满足要求
               while s >= target:
                   #计算subarray的长度
                   l = j - i + 1
                   #对比subarray的长度,并取最小的
                   res = min(res, l)
                   #更新subarray相加的值,并往前进一位
                   s = s - nums[i]
                   i = i + 1
           if res == float('inf'):
               return 0
           else:
               return res
   ~~~

   

## 59 Spiral Matrix II

Problem: https://leetcode.com/problems/spiral-matrix-ii/description/

Solution: https://programmercarl.com/0059.%E8%9E%BA%E6%97%8B%E7%9F%A9%E9%98%B5II.html

for循环注意边界条件(循环不变量原则) O(N^2)：

~~~python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        # 初始位置i,j都等于0
        start_i = 0
        start_j = 0
        count = 1
        #每一圈需要减去的值
        offset = 1
        #创建n*n数组
        nums = [[0 for i in range(n)] for j in range(n)]
        for i in range(n//2 + 1):
            for j in range(start_i, n - offset): # 从左至右，左闭右开
                count += 1
            for i in range(start_j,n - offset): # 从上至下
                nums[i][n - offset] = count
                count += 1
            for j in range(n - offset,start_j, -1): # 从右至左
                nums[n - offset][j] = count
                count += 1
            for i in range(n - offset,start_i, -1): # 从下至上
                nums[i][start_j] = count
                count += 1
            start_i += 1   # 更新起始点
            start_j += 1
            offset += 1
        if n % 2 == 1:
            # n为奇数时，填充中心点
            nums[n // 2 ][n // 2] = count
        return nums
~~~

## 数组总结

https://programmercarl.com/%E6%95%B0%E7%BB%84%E6%80%BB%E7%BB%93%E7%AF%87.html

<img src="https://code-thinking-1253855093.file.myqcloud.com/pics/%E6%95%B0%E7%BB%84%E6%80%BB%E7%BB%93.png" alt="img" style="zoom:200%;" />