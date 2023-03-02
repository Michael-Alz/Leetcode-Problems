# Day 03 | 203 Remove Linked List Elements | 707 Design Linked List ｜ 206 Reverse Linked List

## 链表理论基础

什么是链表，链表是一种通过指针串联在一起的线性结构，每一个节点由两部分组成，一个是数据域一个是指针域（存放指向下一个节点的指针），最后一个节点的指针域指向null（空指针的意思）。

链表的入口节点称为链表的头结点也就是head。

链表定义：

~~~python
class ListNode:
    def __init__(self, val, next=None):
        self.val = val
        self.next = next
~~~



https://www.programmercarl.com/%E9%93%BE%E8%A1%A8%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E5%8D%95%E9%93%BE%E8%A1%A8

## 203 Remove Linked List Elements

Problem : https://leetcode.com/problems/remove-linked-list-elements/

Solution : https://www.programmercarl.com/0203.%E7%A7%BB%E9%99%A4%E9%93%BE%E8%A1%A8%E5%85%83%E7%B4%A0.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC

删除链表中所有值为给定值的节点。代码使用虚拟头节点来简化操作，遍历链表并删除节点。最后返回头节点。O(n)

~~~python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: ListNode, val: int) -> ListNode:
        dummy_head = ListNode(next=head) #添加一个虚拟节点
        cur = dummy_head
        while(cur.next!=None):
            if(cur.next.val == val):
                cur.next = cur.next.next #删除cur.next节点
            else:
                cur = cur.next
        return dummy_head.next
~~~

## 707 Design Linked List

Problem : https://leetcode.com/problems/design-linked-list/

Solution : https://www.programmercarl.com/0707.%E8%AE%BE%E8%AE%A1%E9%93%BE%E8%A1%A8.html

需要理解每一个node只能用前一个node.next获取， 所以可以用虚拟节点来获取第一个node

**Singly Linked List**：

- Time complexity: O(1)O(1) for addAtHead. O(*k*) for get, addAtIndex, and deleteAtIndex, where k is an index of the element to get, add or delete. O(*N*) for addAtTail.
- Space complexity:O(1) for all operations.

~~~python
class ListNode:
	#创建一个node并指向None
    def __init__(self,data):
        self.val = data
        self.next = None


class MyLinkedList:

    def __init__(self, head = None):
        self.size = 0
        self.head = ListNode(0) #创建虚拟节点
        

    def get(self, index: int) -> int: #这里index是从0开始的，第一个node下标应该是0
        if index < 0 or index >= self.size:
            return -1
        cur = self.head.next #创建一个指向第一个node
        while index:
            cur = cur.next
            index -= 1
        return cur.val
        

    def addAtHead(self, val: int) -> None:
        new_node = ListNode(val) #创建一个新的Node
        #让新节点指向头部
        new_node.next = self.head.next
        #让虚拟节点指向新节点
        self.head.next = new_node
        #增加节点的个数
        self.size += 1
        

        

    def addAtTail(self, val: int) -> None:
        #定义一个dummy node
        cur = self.head
        #寻找尾部node
        while cur.next != None:
            cur = cur.next
        new_node = ListNode(val) #创建一个新的Node
        # #新节点指向None #不需要这步，因为默认创建新节点就指向None
        # new_node.next = None
        #尾部节点指向新节点
        cur.next = new_node
        self.size += 1
        

    def addAtIndex(self, index: int, val: int) -> None:
        #如果index大于size就不添加，如果等于就插入到尾部
        if index > self.size:
            return
        elif index == self.size:
            self.addAtTail(val)
            return
        elif index < 0 :
            self.addAtHead(val)
            return
        new_node = ListNode(val)
        #寻找index前一个node的位置
        pre = self.head
        while index:
            pre = pre.next
            index -= 1
        #把新节点连到index前一个节点next的位置
        new_node.next = pre.next
        #把index前一个节点连到新节点
        pre.next = new_node
        self.size += 1
        

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return
        #找到index前面的节点
        pre = self.head
        while index:
            pre = pre.next
            index -= 1
        #把前面的节点连到要删除节点后面
        pre.next = pre.next.next
        self.size -= 1

        


# Your MyLinkedList object will be instantiated and called as such:
# obj = MyLinkedList()
# param_1 = obj.get(index)
# obj.addAtHead(val)
# obj.addAtTail(val)
# obj.addAtIndex(index,val)
# obj.deleteAtIndex(index)
~~~

Doubly Linked List：

~~~python
# 双链表
# 相对于单链表, Node新增了prev属性
class Node:
    
    def __init__(self, val):
        self.val = val
        self.prev = None
        self.next = None


class MyLinkedList:

    def __init__(self):
        self._head, self._tail = Node(0), Node(0)  # 虚拟节点
        self._head.next, self._tail.prev = self._tail, self._head
        self._count = 0  # 添加的节点数

    def _get_node(self, index: int) -> Node:
        # 当index小于_count//2时, 使用_head查找更快, 反之_tail更快
        if index >= self._count // 2:
            # 使用prev往前找
            node = self._tail
            for _ in range(self._count - index):
                node = node.prev
        else:
            # 使用next往后找
            node = self._head   
            for _ in range(index + 1):
                node = node.next
        return node

    def get(self, index: int) -> int:
        """
        Get the value of the index-th node in the linked list. If the index is invalid, return -1.
        """
        if 0 <= index < self._count:
            node = self._get_node(index)
            return node.val
        else:
            return -1

    def addAtHead(self, val: int) -> None:
        """
        Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
        """
        self._update(self._head, self._head.next, val)

    def addAtTail(self, val: int) -> None:
        """
        Append a node of value val to the last element of the linked list.
        """
        self._update(self._tail.prev, self._tail, val)

    def addAtIndex(self, index: int, val: int) -> None:
        """
        Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted.
        """
        if index < 0:
            index = 0
        elif index > self._count:
            return
        node = self._get_node(index)
        self._update(node.prev, node, val)

    def _update(self, prev: Node, next: Node, val: int) -> None:
        """
            更新节点
            :param prev: 相对于更新的前一个节点
            :param next: 相对于更新的后一个节点
            :param val:  要添加的节点值
        """
        # 计数累加
        self._count += 1
        node = Node(val)
        prev.next, next.prev = node, node
        node.prev, node.next = prev, next

    def deleteAtIndex(self, index: int) -> None:
        """
        Delete the index-th node in the linked list, if the index is valid.
        """
        if 0 <= index < self._count:
            node = self._get_node(index)
            # 计数-1
            self._count -= 1
            node.prev.next, node.next.prev = node.next, node.prev
~~~

## 206 Reverse Linked List

Problem : https://leetcode.com/problems/reverse-linked-list/description/

Solution: https://www.programmercarl.com/0206.%E7%BF%BB%E8%BD%AC%E9%93%BE%E8%A1%A8.html#%E5%8F%8C%E6%8C%87%E9%92%88%E6%B3%95

反转链表：双指针法 O（n）

~~~python
#双指针
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        cur = head   
        pre = None
        while(cur!=None):
            temp = cur.next # 保存一下 cur的下一个节点，因为接下来要改变cur->next
            cur.next = pre #反转
            #更新pre、cur指针
            pre = cur
            cur = temp
        return pre
~~~

递归: O(n) 根据双指针的定义可以写出递归，更新pre和cur的值用于第二次递归的input 

~~~python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        def reverse(cur,pre):
            if cur == None: 
                return pre
            temp = cur.next
            cur.next = pre
            return reverse(temp,cur) #更新pre、cur指针
        return reverse(head,None) #带入初始值
~~~

