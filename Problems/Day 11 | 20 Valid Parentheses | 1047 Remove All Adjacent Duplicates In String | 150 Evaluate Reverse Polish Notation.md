# Day 11 | 20 Valid Parentheses | 1047 Remove All Adjacent Duplicates In String | 150 Evaluate Reverse Polish Notation

## 20 Valid Parentheses

Problem : https://leetcode.com/problems/valid-parentheses/description/

Solution: https://programmercarl.com/0020.%E6%9C%89%E6%95%88%E7%9A%84%E6%8B%AC%E5%8F%B7.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

用栈stack实现：

- Time complexity : O(n)
- Space complexity : O(n)

~~~python
class Solution:
    def isValid(self, s: str) -> bool:
        #用栈实现这个功能
        my_stack = []
        #把括号放入dict
        my_dict = {"{":"}","[":"]","(":")"}
        for i in s:
        #把所有需要匹配的右括号括号放入stack
            if i in my_dict.keys():
                my_stack.append(my_dict[i])
            else:
                #当stack不为空并且stack的top与i相等时，把需要匹配的删掉
                if my_stack and my_stack[-1] == i:
                    my_stack.pop()
                #如果发现有不对的括号，可以直接return false
                else:
                    return False
        #当所有需要匹配的括号都匹配到了，stack就为空
        if not my_stack:
            return True
        else:
            return False
~~~

## 1047 Remove All Adjacent Duplicates In String

Problem: https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/

Solution: https://programmercarl.com/1047.%E5%88%A0%E9%99%A4%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%AD%E7%9A%84%E6%89%80%E6%9C%89%E7%9B%B8%E9%82%BB%E9%87%8D%E5%A4%8D%E9%A1%B9.html#%E6%AD%A3%E9%A2%98

用栈stack实现：

- Time complexity : O(N), where N is a string length.
- Space complexity : O(N−D)where D is a total length for all duplicates.

~~~python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        # 用deque实现
        stack = []
        #遍历所有的字母
        for c in s:
            #当stack不为空并且这个字母与stack最后一个元素一致的时候，删除stack里的元素
            if stack and c == stack[-1]:
                stack.pop()
            #当stack为空或者出现不相邻的字母时把字母加入stack
            else:
                stack.append(c)
        return ''.join(stack)
~~~

双指针：

~~~python
# 方法二，使用双指针模拟栈，如果不让用栈可以作为备选方法。
class Solution:
    def removeDuplicates(self, s: str) -> str:
        res = list(s)
        slow = fast = 0
        length = len(res)

        while fast < length:
            # 如果一样直接换，不一样会把后面的填在slow的位置
            res[slow] = res[fast]
            
            # 如果发现和前一个一样，就退一格指针
            if slow > 0 and res[slow] == res[slow - 1]:
                slow -= 1
            else:
                slow += 1
            fast += 1
            
        return ''.join(res[0: slow])
~~~

## 150 Evaluate Reverse Polish Notation

Problem: https://leetcode.com/problems/evaluate-reverse-polish-notation/

Solution: https://programmercarl.com/0150.%E9%80%86%E6%B3%A2%E5%85%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F%E6%B1%82%E5%80%BC.html

用栈实现运算：

- Time Complexity : O(n)
- Space Complexity : O(n)

~~~python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        #用stack实现
        stack = []
        symbols = ['+', '-', '*', '/']
        #把tokens里的放入stack，如果是数字就append，
        #如果是符号就和stack前两位进行计算，然后把计算结果放入stack
        for i in tokens:
            if i not in symbols:
                stack.append(i)
            else:
                num2 = int(stack.pop())
                num1 = int(stack.pop())
                if i == '+':
                    cal = num1 + num2
                elif i == '-':
                    cal = num1 - num2
                elif i == '*':
                    cal = num1 * num2
                elif i == '/':
                    cal = int(num1 / num2)
                stack.append(cal)
        return int(stack[0])
~~~

