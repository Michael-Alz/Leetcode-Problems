# Day 09 | 28 Find the Index of the First Occurrence in a String | 459 Repeated Substring Pattern

## 28 Find the Index of the First Occurrence in a String

Problem: https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/description/

Solution: https://www.programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/editorial/

Sliding Window（Brute-Force）:

Time complexity: O(nm)

Space complexity: O(1)

~~~python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int: 
        #滑动窗口，跟每一个做对比，最多需要匹配 h - n + 1 次
        h = len(haystack)
        n = len(needle)
        for i in range(h - n + 1):
            if haystack[i: i + n] == needle:
                return i
        return -1
~~~

Knuth–Morris–Pratt Algorithm（KMP):

Time complexity: O(n)

Space complexity: O(m)

Next数组：https://www.bilibili.com/video/BV16X4y137qw/?spm_id_from=333.337.search-card.all.click

~~~python
def strStr(self, haystack: str, needle: str) -> int:
        a = len(needle)
        b = len(haystack)
        if a == 0:
            return 0
        i = 0 #主串中的指针
				j = 0 #子串中的指针
        next = self.getnext(a, needle)
        while(i < b and j < a):
            if j == -1 or needle[j] == haystack[i]: #字符匹配，两个指针后移
                i += 1
                j += 1
            else: #匹配失败，根据next数组跳过一些字符串
                j = next[j]
        if j == a: #匹配成功，j走到字串最后
            return i-j 
        else:
            return -1

#定义next数组, a为needle的长度
    def getnext(self, a, needle):
        next = ['' for i in range(a)]
        j, k = 0, -1
        next[0] = k
        while(j < a-1):
            if k == -1 or needle[k] == needle[j]:
                k += 1
                j += 1
                next[j] = k
            else: #前后缀不同了，向前回溯，向前回溯的位置是next[k]的值，
									#k+1的值可以直接跟这个比较得出结果，如果不同接着回溯直到 k = -1 或者匹配到了
                k = next[k]
        return next
~~~

## 459 Repeated Substring Pattern

Problem: https://leetcode.com/problems/repeated-substring-pattern/description/

Solution: https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0459.%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2.md

KMP：

Time complexity: O(n)

Space complexity: O(n)

假设字符串s使用多个重复子串构成（这个子串是最小重复单位），重复出现的子字符串长度是x，所以s是由n * x组成。

因为字符串s的最长相同前后缀的长度一定是不包含s本身，所以 最长相同前后缀长度必然是m * x，而且 n - m = 1，（这里如果不懂，看上面的推理）

所以如果 nx % (n - m)x = 0，就可以判定有重复出现的子字符串。

next 数组记录的就是最长相同前后缀 [字符串：KMP算法精讲](https://programmercarl.com/0028.实现strStr.html) 这里介绍了什么是前缀，什么是后缀，什么又是最长相同前后缀)， 如果 next[len - 1] != -1，则说明字符串有最长相同的前后缀（就是字符串里的前缀子串和后缀子串相同的最长长度）。

最长相等前后缀的长度为：next[len - 1] + 1。(这里的next数组是以统一减一的方式计算的，因此需要+1，两种计算next数组的具体区别看这里：[字符串：KMP算法精讲](https://programmercarl.com/0028.实现strStr.html))

数组长度为：len。

如果len % (len - (next[len - 1] + 1)) == 0 ，则说明数组的长度正好可以被 (数组长度-最长相等前后缀的长度) 整除 ，说明该字符串有重复的子字符串。

**数组长度减去最长相同前后缀的长度相当于是第一个周期的长度，也就是一个周期的长度，如果这个周期可以被整除，就说明整个数组就是这个周期的循环。**

![image-20230309161953579](/Users/michaelan/Library/Application Support/typora-user-images/image-20230309161953579.png)

next[len - 1] = 7，next[len - 1] + 1 = 8，8就是此时字符串asdfasdfasdf的最长相同前后缀的长度。

(len - (next[len - 1] + 1)) 也就是： 12(字符串的长度) - 8(最长公共前后缀的长度) = 4， 4正好可以被 12(字符串的长度) 整除，所以说明有重复的子字符串（asdf）。

~~~python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:  
        if len(s) == 0:
            return False
        nxt = [0] * len(s)
        self.getNext(nxt, s)
        if nxt[-1] != 0 and len(s) % (len(s) - nxt[-1]) == 0:
            return True
        return False
    
    def getNext(self, nxt, s):
        nxt[0] = 0
        j = 0
        for i in range(1, len(s)):
            while j > 0 and s[i] != s[j]:
                j = nxt[j - 1]
            if s[i] == s[j]:
                j += 1
            nxt[i] = j
        return nxt
~~~

## **字符串总结** 

https://programmercarl.com/%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%80%BB%E7%BB%93.html

##  **双指针回顾** 

https://programmercarl.com/%E5%8F%8C%E6%8C%87%E9%92%88%E6%80%BB%E7%BB%93.html