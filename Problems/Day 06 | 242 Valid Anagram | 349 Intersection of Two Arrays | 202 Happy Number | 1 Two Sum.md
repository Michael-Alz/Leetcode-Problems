# Day 06 | 242 Valid Anagram | 349 Intersection of Two Arrays | 202 Happy Number | 1 Two Sum

## 哈希表理论基础

https://github.com/youngyangyang04/leetcode-master/blob/master/problems/%E5%93%88%E5%B8%8C%E8%A1%A8%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.md

## 242 Valid Anagram

Problem:https://leetcode.com/problems/valid-anagram/

Solution: https://www.programmercarl.com/0242.%E6%9C%89%E6%95%88%E7%9A%84%E5%AD%97%E6%AF%8D%E5%BC%82%E4%BD%8D%E8%AF%8D.html#%E6%80%9D%E8%B7%AF

使用哈希表：时间复杂度为O(n)，空间上因为定义是的一个常量大小的辅助数组，所以空间复杂度为O(1)

定义一个数组叫做record用来上记录字符串s里字符出现的次数。

需要把字符映射到数组也就是哈希表的索引下标上，**因为字符a到字符z的ASCII是26个连续的数值，所以字符a映射为下标0，相应的字符z映射为下标25。**

再遍历 字符串s的时候，**只需要将 s[i] - ‘a’ 所在的元素做+1 操作即可，并不需要记住字符a的ASCII，只要求出一个相对数值就可以了。** 这样就将字符串s中字符出现的次数，统计出来了。

那看一下如何检查字符串t中是否出现了这些字符，同样在遍历字符串t的时候，对t中出现的字符映射哈希表索引上的数值再做-1的操作。

那么最后检查一下，**record数组如果有的元素不为零0，说明字符串s和t一定是谁多了字符或者谁少了字符，return false。**

最后如果record数组所有元素都为零0，说明字符串s和t是字母异位词，return true。

~~~python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        #创建一个26位的Hash Table
        record = [0] * 26
        #遍历s
        for i in s:
            #计算字母出现了多少次，ord（）返回的是十进制，由于字母是连续的 a -> 97， b -> 98..., 
            #所以可以用这个来定位指定字母在哈希表里的位置
            record[ord(i) - ord('a')] += 1
        for i in t:
            #遍历t，把出现的字母从哈希表里次数减去
            record[ord(i) - ord('a')] -= 1
        for i in record:
            #遍历哈希表，只要发现有一位字母不等于零，返回false
            if i != 0:
                return False
        return True
~~~



## 349 Intersection of Two Arrays

Problem:https://leetcode.com/problems/intersection-of-two-arrays/

Solution: https://www.programmercarl.com/0349.%E4%B8%A4%E4%B8%AA%E6%95%B0%E7%BB%84%E7%9A%84%E4%BA%A4%E9%9B%86.html#_349-%E4%B8%A4%E4%B8%AA%E6%95%B0%E7%BB%84%E7%9A%84%E4%BA%A4%E9%9B%86

这道题目，主要要学会使用一种哈希数据结构：unordered_set，这个数据结构可以解决很多类似的问题。

注意题目特意说明：**输出结果中的每个元素一定是唯一的，也就是说输出的结果的去重的， 同时可以不考虑输出结果的顺序**

这道题用暴力的解法时间复杂度是O(n^2)，那来看看使用哈希法进一步优化。

那么用数组来做哈希表也是不错的选择，例如[242. 有效的字母异位词(opens new window)](https://programmercarl.com/0242.有效的字母异位词.html)

但是要注意，**使用数组来做哈希的题目，是因为题目都限制了数值的大小。**

而这道题目没有限制数值的大小，就无法使用数组来做哈希表了。

**而且如果哈希值比较少、特别分散、跨度非常大，使用数组就造成空间的极大浪费。**

此时就要使用另一种结构体了，set :

![set哈希法](https://code-thinking-1253855093.file.myqcloud.com/pics/20220707173513.png)

- Time complexity: O(n+m), where n and m are the lengths of nums1 and nums2 respectively.
- Space complexity:O(min(n,m)), where n and m are the lengths of nums1 and nums2 respectively.

~~~python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        #用set：先把一个数组放入set进行去重处理
        set1 = set(nums1)
        set0 = set()
        #遍历第二个数组，把相同的放入set0，set0会自动去重
        for i in nums2:
            if i in set1:
                set0.add(i)
        return list(set0)
~~~

## 202 Happy Number



Problem:https://leetcode.com/problems/happy-number/

Solution: https://programmercarl.com/0202.%E5%BF%AB%E4%B9%90%E6%95%B0.html

This document provides a solution to the LeetCode problem "Happy Number" using a **HashSet**. The solution involves calculating the sum of the squares of each digit in a number, and checking if the resulting sum is equal to 1 or if it repeats in a cycle. The code is implemented in Python.

使用哈希法，来判断这个sum是否重复出现，如果重复了就是return false， 否则一直找到sum为1为止。

Time complexity :O(logN)

Space complexity :O(logN)

~~~python
class Solution:
    def isHappy(self, n: int) -> bool:
        # 先取得每一个数的每一位数字,再求平方和
        def calculate_happy(num):
            sum_ = 0
            while num:
        #从个位的平方开始取，对n模10，在把得出的位数平凡
                sum_ = sum_ + (num % 10) ** 2
        #对n除10向下取整
                num = num // 10 
            #获得取得的快乐数
            return sum_
        sum_set = set()
        #把每次的sum_放入set里
        while True:
            # 计算快乐数
            n = calculate_happy(n)
        #n等于1的时候返回True，说明该数字是快乐数
            if n == 1:
                return True
        #如果出现一样的数字，说明出现死循环里，说明不是快乐数
            if n in sum_set:
                return False
            else:
        #把数字加入set里
                 sum_set.add(n)
~~~



## 1 Two Sum

Problem:https://leetcode.com/problems/two-sum/

Solution: https://www.programmercarl.com/0001.%E4%B8%A4%E6%95%B0%E4%B9%8B%E5%92%8C.html#%E6%80%9D%E8%B7%AF

暴力解法，遍历两遍list：

Time :O(n^2)

Space: O(1)

~~~python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        n = len(nums)
        for i in range(n):
            for j in range(1 + i,n):
                #每次要从i后面的元素开始遍历
                if nums[i] + nums[j] == target:
                    return [i,j]
~~~

哈希表：

那么判断元素是否出现，这个元素就要作为key，所以数组中的元素作为key，有key对应的就是value，value用来存下标。

所以 map中的存储结构为 {key：数据元素，value：数组元素对应的下标}。

在遍历数组的时候，只需要向map去查询是否有和目前遍历元素比配的数值，如果有，就找到的匹配对，如果没有，就把目前遍历的元素放进map中，因为map存放的就是我们访问过的元素。

Time :O(n^2)

Space: O(1)

~~~python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        #用python字典哈希表
        record = dict()
        n = len(nums)
        #遍历一遍list，把元素加入字典（去重）
        for i in range(n):
        #定义match，如果发现match在哈希表里，用key返回index就行
            match = target - nums[i]
            if match in record.keys():
                return [i , record[match]]
            #用数字做key，index做value，这样找到match可以
            #直接用key直接返回index
            record[nums[i]] = i
~~~

