# Day 10 | 225 Implement Stack using Queues | 232 Implement Queue using Stacks

## 栈和队列

### 栈（Stack）

栈，它是一种运算受限的线性表。其限制是仅允许在表的一端进行插入和删除运算。这一端被称为栈顶，相对地，把另一端称为栈底。向一个栈插入新元素又称作进栈、入栈或压栈，它是把新元素放到栈顶元素的上面，使之成为新的栈顶元素；从一个栈删除元素又称作出栈或退栈，它是把栈顶元素删除掉，使其相邻的元素成为新的栈顶元素。

由于使用顺序表的栈易于实现，所以许多地方都默认使用顺序表构建栈，但如果你想用链表实现也不是不可以。以下为一个基本的基于顺序表的栈实现

~~~python
class Stack:
    def __init__(self):
        self.__data = []

    @property
    def length(self):
        return len(self.__data)

    @property
    def top(self):
        """
        仅返回栈顶元素而不出栈, 若栈为空则返回None
        """
        return None if self.length == 0 else self.__data[-1]

    def push(self, data):
        self.__data.append(data)

    def pop(self):
        return self.__data.pop()
~~~

### 队列（Queue）

队列，是先进先出的线性表。队列只允许在尾部进行插入操作，在头部进行删除操作。由于在头部进行删除操作对于顺序表而言，开销较大，所以往往使用链表实现。

~~~python
class Node:

    def __init__(self, data=None):
        self.data = data
        self.next: Node = None

    def __str__(self):
        return f"[{self.data}]"


class Queue:

    def __init__(self):
        self.__head = Node()
        self.__end = self.__head
        self.length = 0

    @property
    def head(self):
        return None if self.__head.next is None else self.__head.next.data

    def append(self, data):
        self.__end.next = Node(data)
        self.__end = self.__end.next
        self.length += 1

    def poll(self):
        if self.length == 0:
            raise IndexError("Queue is empty")
        data = self.__head.next.data
        self.__head = self.__head.next
        self.length -= 1
        return data
~~~

## 232 Implement Queue using Stacks

Problem : https://leetcode.com/problems/implement-queue-using-stacks/

Solution : https://programmercarl.com/0232.%E7%94%A8%E6%A0%88%E5%AE%9E%E7%8E%B0%E9%98%9F%E5%88%97.html#%E6%80%9D%E8%B7%AF

在push数据的时候，只要数据放进输入栈就好，**但在pop的时候，操作就复杂一些，输出栈如果为空，就把进栈数据全部导入进来（注意是全部导入）**，再从出栈弹出数据，如果输出栈不为空，则直接从出栈弹出数据就可以了。

最后如何判断队列为空呢？**如果进栈和出栈都为空的话，说明模拟的队列为空了。**

~~~python
class MyQueue:

    def __init__(self):
        #初始化两个list当作stack
        self.stack_in = []
        self.stack_out = []
        
    def push(self, x: int) -> None:
        #有元素来就放入stack_in
        return self.stack_in.append(x)

    def pop(self) -> int:
        #当没有元素可以删除时，直接返回None
        if self.empty():
            return None 
        #这样写的意思是当stack.out不为空时，if后面判断为True
        if self.stack_out:
            return self.stack_out.pop()
        else: #当stack_out为空的时候，把所有的stack_in元素放入stack_out
            while self.stack_in: #当stack_in不为空
                #从in的最后一个加入out，可以用list.pop()
                self.stack_out.append(self.stack_in.pop())
            return self.stack_out.pop()

        
    def peek(self) -> int:
        #可以直接用上面的self.pop()获得第一个元素，再把去掉的元素放入stack_out的尾部
        ans = self.pop()
        self.stack_out.append(ans)
        return ans
        

    def empty(self) -> bool:
        #当in或者out有元素时，queue不为空
        return not (self.stack_in or self.stack_out)
        
        


# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
~~~



## 225 Implement Stack using Queues

Problem: https://leetcode.com/problems/implement-stack-using-queues/

Solution: https://programmercarl.com/0225.%E7%94%A8%E9%98%9F%E5%88%97%E5%AE%9E%E7%8E%B0%E6%A0%88.html

用一个队列实现：

一个队列在模拟栈弹出元素的时候只要将队列头部的元素（除了最后一个元素外） 重新添加到队列尾部，此时再去弹出元素就是栈的顺序了。

~~~python
class MyStack:
    # from collections import deque
    def __init__(self):
        #使用一个队列实现
        self.my_queue = deque()
    def push(self, x: int) -> None:
         self.my_queue.append(x)

    def pop(self) -> int:
        if self.empty():
            return None
        #从左边出队再从右边进队，把最后一个元素去除之后停止
        for i in range(len(self.my_queue) - 1):
            self.my_queue.append(self.my_queue.popleft())
        return  self.my_queue.popleft()
    
    def top(self) -> int:
        if self.empty():
            return None
        return self.my_queue[-1]
        
        

    def empty(self) -> bool:
        return len(self.my_queue) == 0
        


# Your MyStack object will be instantiated and called as such:
# obj = MyStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.empty()
~~~

