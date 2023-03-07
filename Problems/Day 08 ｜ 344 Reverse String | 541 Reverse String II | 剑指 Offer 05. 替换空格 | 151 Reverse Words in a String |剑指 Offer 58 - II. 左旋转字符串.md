# Day 08 ｜ 344 Reverse String | 541 Reverse String II | 剑指 Offer 05. 替换空格 | 151 Reverse Words in a String |剑指 Offer 58 - II. 左旋转字符串

	## 344 Reverse String

Problem: https://leetcode.com/problems/reverse-string/description/

Solution: https://www.programmercarl.com/0344.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

双指针：

- Time complexity : O(N)to swap N/2N element.
- Space complexity : O(1), it's a constant space solution.

~~~python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        #双指针法往中间移动
        left = 0
        right = len(s) - 1
        while left < right:
         #需要一个temp存左边的值
            temp = s[left] 
         #交换left和right的值
            s[left] = s[right]
            s[right] = temp
            left += 1
            right -= 1
~~~



~~~python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left, right = 0, len(s) - 1
        
        # 该方法已经不需要判断奇偶数，经测试后时间空间复杂度比用 for i in range(right//2)更低
        # 推荐该写法，更加通俗易懂
        while left < right:
            s[left], s[right] = s[right], s[left]
            left += 1
            right -= 1
~~~

## 541 Reverse String II

Problem: https://leetcode.com/problems/reverse-string-ii/

Solution: https://www.programmercarl.com/0541.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2II.html

直接按照规则模拟就行，python可以用range(start, end, step)

- Time Complexity: O(N)
- Space Complexity: O(N)

~~~python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        #自己写reverse function
        def my_reverse(string):
            left, right = 0 , len(string) - 1            
            while left < right:
                string[left] , string[right] = string[right],string[left]
                left += 1
                right -= 1
            return string
        #需要把s转化为list才能用上面的function
        res = list(s)
        #用range控制移动的步数, range(start, end, step)
        #range为左闭区间右开区间，不包含右边的数字,list[a:b]也是，不包含b的index
        for i in range(0, len(res) , 2*k):
            print(i)
            print(res)
            res[i:i + k] = my_reverse(res[i:i + k])
        return ''.join(res)
~~~

## 剑指 Offer 05. 替换空格

Problem: https://leetcode.cn/problems/ti-huan-kong-ge-lcof/

Solution: https://www.programmercarl.com/%E5%89%91%E6%8C%87Offer05.%E6%9B%BF%E6%8D%A2%E7%A9%BA%E6%A0%BC.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

双指针：当str可以原地修改的时候可以用这种方法，不用创造新的空间

Python：Time: O（n）Space: O(n)

C++: Time: O(n)  Space: O(1)

~~~python
class Solution:
    def replaceSpace(self, s: str) -> str:
        counter = s.count(' ')
        
        res = list(s)
        # 每碰到一个空格就多拓展两个格子，1 + 2 = 3个位置存’%20‘
        res.extend([' '] * counter * 2)
        
        # 原始字符串的末尾，拓展后的末尾
        left, right = len(s) - 1, len(res) - 1
        
        while left >= 0:
            if res[left] != ' ':
                res[right] = res[left]
                right -= 1
            else:
                # [right - 2, right), 左闭右开
                res[right - 2: right + 1] = '%20'
                right -= 3
            left -= 1
        return ''.join(res)
~~~

创建新的空间解法：

Time: O（n）Space: O(n)

~~~python
class Solution:
    def replaceSpace(self, s: str) -> str:
        l = list(s)
        r = 0
        while r <= len(s) - 1:
            if l[r] == ' ':
                l[r] = '%20'
            r += 1
        return ''.join(l)
~~~

## 151 Reverse Words in a String

Problem: https://leetcode.com/problems/reverse-words-in-a-string/

Solution: https://www.programmercarl.com/0151.%E7%BF%BB%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%87%8C%E7%9A%84%E5%8D%95%E8%AF%8D.html

题思路如下：

- 移除多余空格
- 将整个字符串反转
- 将每个单词反转

举个例子，源字符串为："the sky is blue "

- 移除多余空格 : "the sky is blue"
- 字符串反转："eulb si yks eht"
- 单词反转："blue is sky the"

这样我们就完成了翻转字符串里的单词。

- Time complexity: O(N), where N is a number of characters in the input string.
- Space complexity: O(N), to store the result of split by spaces.

~~~python
class Solution:
    #首先移除多余的空格,可以直接遍历string，把结果放到list里，就不用list（）函数了
    def remove_space(self,l):
        left = 0
        right = len(l) - 1
        #去除前面多余的空格
        while left < right and l[left] == ' ':
            left += 1
        #去除后面多余的空格
        while left < right and l[right] == ' ':
            right -= 1
        #创建一个空的list来获取去除完空格的list
        temp = []
        while left <= right:
        #把不为空格的放入temp
            if l[left] != ' ':
                temp.append(l[left])
        #当前位置为空格但是相邻的上一个位置不是空格，则该空格是合理的
            elif temp[-1] != ' ':
                temp.append(l[left])
            left += 1
        return temp

    #反转整个字符串
    def reverse_list(self, l , left , right):
        while left < right:
            l[left] , l[right] = l[right] , l[left]
            left += 1
            right -= 1
        return None

    #反转每个单词
    def reverse_word(self, l):
        start = 0
        end = 0
        n = len(l)
        while start < n:
            #找到单词的结尾，如果字母后一个为空格，则为结尾字母
            while end < n and l[end] != ' ':
                end += 1
            #反转当前单词
            self.reverse_list(l,start,end - 1)
            start = end + 1
            end += 1
        return None
    
    #反转题目要求的string

    def reverseWords(self, s):     #测试用例："the sky is blue"
        l = self.remove_space(s) #输出：['t', 'h', 'e', ' ', 's', 'k', 'y', ' ', 'i', 's', ' ', 'b', 'l', 'u', 'e'
        self.reverse_list(l,0, len(l)-1)#输出：['e', 'u', 'l', 'b', ' ', 's', 'i', ' ', 'y', 'k', 's', ' ', 'e', 'h', 't']
        self.reverse_word(l)    #输出：['b', 'l', 'u', 'e', ' ', 'i', 's', ' ', 's', 'k', 'y', ' ', 't', 'h', 'e']
        return ''.join(l)  #输出：blue is sky the

~~~

## 剑指 Offer 58 - II. 左旋转字符串

Problem: https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/

Solution: https://www.programmercarl.com/%E5%89%91%E6%8C%87Offer58-II.%E5%B7%A6%E6%97%8B%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

步骤为：

1. 反转区间为前n的子串
2. 反转区间为n到末尾的子串
3. 反转整个字符串

最后就可以达到左旋n的目的，而不用定义新的字符串，完全在本串上操作。

Time complexity ： O(n)

Space complexity : O(n)

~~~python
class Solution:
    def reverseLeftWords(self, s: str, n: int) -> str:
        s = list(s)
        #先把前n个元素反转(reversed（）只会返回一个迭代器，所以要加list（）获得反转后的list)
        s[0 : n] = list(reversed(s[0:n]))
        #把后面剩下的元素反转
        s[n  : ] = list(reversed(s[n  :]))
				#整体反转就能得到答案
        s.reverse()
        return ''.join(s)
~~~

