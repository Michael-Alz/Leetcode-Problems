# Day 07 | 383 Ransom Note | 454 4Sum II | 15 3Sum | 18 4Sum

## 383 Ransom Note

Problem: https://leetcode.com/problems/ransom-note/description/

Solution: https://www.programmercarl.com/0383.%E8%B5%8E%E9%87%91%E4%BF%A1.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

因为题目所只有小写字母，那可以采用空间换取时间的哈希策略， 用一个长度为26的数组还记录magazine里字母出现的次数。

然后再用ransomNote去验证这个数组是否包含了ransomNote所需要的所有字母。

Time Complexity : O(m)

Space Complexity : O(k)/ O(1)

~~~python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        #因为只有小写字母，所以可以创建26位哈希表
        l = 26 *[0]
        #把rmagazine里的字母放入哈希表并统计每个的数量
        for i in magazine:
            l[ord(i) - ord('a')] += 1
        #把ransomNote里的字母与l里比较
        for j in ransomNote:
        #如果当前可用字母数量为0直接 return False
            if l[ord(j) - ord('a')] == 0 :
                return False
        #把可用的字母数量减去1
            else:
                l[ord(j) - ord('a')] -= 1
        return True
~~~

暴力解法：

O(m*n)

O(m)

~~~python
def canConstruct(self, ransomNote: str, magazine: str) -> bool:
    # For each character, c,  in the ransom note.
    for c in ransomNote:
        # If there are none of c left in the String, return False.
        if c not in magazine:
            return False
        # Find the index of the first occurrence of c in the magazine.
        location = magazine.index(c)
        # Use splicing to make a new string with the characters 
        # before "location" (but not including), and the characters 
        #after "location". 
        magazine = magazine[:location] + magazine[location + 1:]
    # If we got this far, we can successfully build the note.
    return True
~~~

## 454 4Sum II

Problem:https://leetcode.com/problems/4sum-ii/

Solution:https://www.programmercarl.com/0454.%E5%9B%9B%E6%95%B0%E7%9B%B8%E5%8A%A0II.html

本题解题步骤：

1. 首先定义 一个unordered_map，key放a和b两数之和，value 放a和b两数之和出现的次数。
2. 遍历大A和大B数组，统计两个数组元素之和，和出现的次数，放到map中。
3. 定义int变量count，用来统计 a+b+c+d = 0 出现的次数。
4. 在遍历大C和大D数组，找到如果 0-(c+d) 在map中出现过的话，就用count把map中key对应的value也就是出现次数统计出来。
5. 最后返回统计值 count 就可以了

Space: O(n^2)

Time : O(n^2)

~~~python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        #定义一个dict存储nums1+nums2的值出现的次数
        my_map = dict()
        #先遍历1+2
        for n1 in nums1:
            for n2 in nums2:
                if n1 + n2 in my_map:
                    my_map[n1 + n2] += 1
                else:
                    my_map[n1 + n2] = 1
        #再遍历3和4，记录 -3 - 4 的值，与dict里的做对比，如果发现
        #符合等于0的，返回dict里的value, 因为每有一个-3 - 4 == 1+2，
        #就有value个数符合题目
        count = 0
        for n3 in nums3:
            for n4 in nums4:
                if -n3 - n4 in my_map:
                    count += my_map[-n3 - n4]
        return count
~~~

## 15 3Sum

Problem: https://leetcode.com/problems/3sum/

Solution: https://www.programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html#%E5%93%88%E5%B8%8C%E8%A7%A3%E6%B3%95

双指针法

Time : O(n^2)

Space : O(n) or O(logn) depending on sorted()

首先将数组排序，然后有一层for循环，i从下标0的地方开始，同时定一个下标left 定义在i+1的位置上，定义下标right 在数组结尾的位置上。

依然还是在数组中找到 abc 使得a + b +c =0，我们这里相当于 a = nums[i]，b = nums[left]，c = nums[right]。

接下来如何移动left 和right呢， 如果nums[i] + nums[left] + nums[right] > 0 就说明 此时三数之和大了，因为数组是排序后了，所以right下标就应该向左移动，这样才能让三数之和小一些。

如果 nums[i] + nums[left] + nums[right] < 0 说明 此时 三数之和小了，left 就向右移动，才能让三数之和大一些，直到left与right相遇为止。

~~~python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
		#需要对nums进行排序
        n = sorted(nums)
        length = len(nums)
        result = []
        for i in range(length):
		#如果最小的数字大于0，直接return答案，证明没有结果
            if n[i] > 0:
                break
		#对i进行去重，注意应该与i前面的数进行比较，因为i前面的数字已经进行过后面的寻找left和right，
		#所以如果与前面的数字相等直接跳过
            if i > 0 and n[i-1] == n[i]:
                continue
		#left是i右边的数字，right是最后的数字
            left = i + 1
            right = length - 1
		#注意如果等于的话就不符合，因为left和right指向一个数字
            while left < right:
                total = n[i] + n[left] + n[right]
		#如果比0小，说明total小了需要找大的数，把left往右移一为
                if total < 0:
                    left += 1
		#同理把right往左移一位
                elif total > 0:
                    right -= 1
                else:
		#找到合适的进行去重
                    result.append([n[i],n[left],n[right]])
		#去重逻辑是，如果left后面一位的数一样，说明重复，往右跳过去寻找下一个
                    while left < right and n[left] ==  n[left + 1]:
                        left += 1
		#同理right是往左跳过去
                    while left < right and n[right] ==  n[right - 1]:
                        right -= 1
		#找到相同的可以直接往中间进两位：left往右，right往左
                    left += 1
                    right -= 1
        return result
~~~

## 18 4Sum

Problem: https://leetcode.com/problems/4sum/description/

Solution: https://www.programmercarl.com/0018.%E5%9B%9B%E6%95%B0%E4%B9%8B%E5%92%8C.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

**[15.三数之和 (opens new window)](https://programmercarl.com/0015.三数之和.html)**的双指针解法是一层for循环num[i]为确定值，然后循环内有left和right下标作为双指针，找到nums[i] + nums[left] + nums[right] == 0。

四数之和的双指针解法是两层for循环nums[k] + nums[i]为确定值，依然是循环内有left和right下标作为双指针，找出nums[k] + nums[i] + nums[left] + nums[right] == target的情况，三数之和的时间复杂度是O(n^2)，四数之和的时间复杂度是O(n^3) 。

那么一样的道理，五数之和、六数之和等等都采用这种解法。

Time Complexity: O(n^(k−1)) or O(n^3) for 4Sum

Space Complexity: O(n)

~~~python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        n = sorted(nums)
        size = len(nums)
        result = []
        for i in range(size):
            # if n[i] > 0 and n[i] > target and target > 0:
            #     break 
            if i > 0 and n[i - 1] == n[i]:
                continue
            # j是在i的后一位，所以i+1
            for j in range(i + 1,size):
                # if n[i] + n[j] > 0 and n[i] + n[j] > target and target > 0:
                #     break
                if j > 1 + i and n[j - 1] == n[j]:
                    continue
                right = size - 1
                left = j + 1
                while left < right:
                    total = n[i] + n[j] + n[left] + n[right] 
                    if total < target:
                        left += 1
                    elif total > target:
                        right -= 1
                    else:
                        result.append([n[i], n[j],n[left], n[right]])
                        while left < right and n[left] == n[left + 1]:
                            left += 1
                        while left < right and n[right] == n[right - 1]:
                            right -= 1
                        left += 1
                        right -= 1
        return result
~~~

