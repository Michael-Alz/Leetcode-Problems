# Day 04 ｜24 Swap Nodes in Pairs ｜19 Remove Nth Node From End of List ｜ 160 Intersection of Two Linked Lists ｜142 Linked List Cycle II

## 24. Swap Nodes in Pairs

Problem : https://leetcode.com/problems/swap-nodes-in-pairs/

Solution : https://programmercarl.com/0024.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html

初始时，cur指向虚拟头结点，然后进行如下三步：

![24.两两交换链表中的节点1](https://code-thinking.cdn.bcebos.com/pics/24.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B91.png)

操作之后，链表如下：

![24.两两交换链表中的节点2](https://code-thinking.cdn.bcebos.com/pics/24.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B92.png)

看这个可能就更直观一些了：

![24.两两交换链表中的节点3](https://code-thinking.cdn.bcebos.com/pics/24.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B93.png)

~~~python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(next = head) 
        cur = dummy #创建一个虚拟节点指向头部
        while (cur.next and cur.next.next) != None: #当后两个不为空的时候才能交换
        #创建两个临时节点保存下一个和下下个
            temp1 = cur.next
            temp2 = cur.next.next.next
            #第一步把第虚拟节点指向第二个节点
            cur.next = cur.next.next
            #第二步把第二个节点指向第一个节点
            cur.next.next = temp1
            #第三步把第一个节点指向第三个节点
            cur.next.next.next = temp2
            #更新到下一组
            cur = cur.next.next
        return dummy.next
~~~

## 19. Remove Nth Node From End of List

Problem : https://leetcode.com/problems/swap-nodes-in-pairs/

Solution : https://programmercarl.com/0019.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.html#_19-%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACn%E4%B8%AA%E8%8A%82%E7%82%B9

快慢指针，让快的先走n步，然后快慢一起走知道快指针走到最后一个，这时慢指针在要删除的前一个节点

![img](https://code-thinking.cdn.bcebos.com/pics/19.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.png)

![img](https://code-thinking.cdn.bcebos.com/pics/19.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B91.png)

![img](https://code-thinking.cdn.bcebos.com/pics/19.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B92.png)

![img](https://code-thinking.cdn.bcebos.com/pics/19.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B93.png)

~~~python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy = ListNode()
        dummy.next = head
        #定义两个指针
        fast = dummy
        slow = dummy
        #快指针先走n步
        while n > 0:
            fast = fast.next
            n -= 1
        #再让快慢指针一起走，快指针走到头，慢指针正好到要删除节点之前
        while fast.next != None:
            fast = fast.next
            slow = slow.next
        #删除倒数第n个节点
        slow.next = slow.next.next
        return dummy.next
~~~

# 160 Intersection of Two Linked Lists

Problem : https://leetcode.com/problems/intersection-of-two-linked-lists/

Solution: https://www.programmercarl.com/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.html#%E6%80%9D%E8%B7%AF

Alternative Method: https://leetcode.com/problems/intersection-of-two-linked-lists/editorial/

先计算两个list的长度，再把他们对齐，最后再遍历：

- 时间复杂度：O(n + m)
- 空间复杂度：O(1)

~~~python
先计算两个list的长度，再把他们对齐，最后再遍历：

- 时间复杂度：O(n + m)
- 空间复杂度：O(1)
~~~

双指针：O(n + m)

如果这种情况，一个指针会遍历 a + c + b，另一个遍历b + c + a

所以定义两个指针，当一个指针到None了换到另一个指针的头

![img](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/592cfbaf-d372-40a2-ac01-8a8c100f1a63/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230304%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230304T041746Z&X-Amz-Expires=86400&X-Amz-Signature=41be5b6f9546b260065d8886d49dd3481dfefe9c358596cb59152fddfd29dd5c&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

~~~python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        l1, l2 = headA, headB# 定义两个指针
        while l1 != l2 : #l1不等于l2的时候遍历两个，如果相等直接返回l1就行
        #遍历l1，如果l1到None了把l1换到headB
            if l1 is None: 
                l1 = headB
            else:
                l1 = l1.next
        #同时遍历l2，如果l2到None了把l2换到headA
            if l2 is None:
                l2  = headA
            else:
                l2 = l2.next
        return l1
~~~

## 142 Linked List Cycle II

Problem: https://leetcode.com/problems/linked-list-cycle-ii/

Solution: https://www.programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html#_142-%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8ii

双指针：如果有环的话，链表会一直循环，快慢指针最终会相遇；如果没有环，快指针会指到None

假设从头结点到环形入口节点 的节点数为x。 环形入口节点到 fast指针与slow指针相遇节点 节点数为y。 从相遇节点 再到环形入口节点节点数为 z。

那么相遇时： slow指针走过的节点数为: `x + y`， fast指针走过的节点数：`x + y + n (y + z)`，n为fast指针在环内走了n圈才遇到slow指针， （y+z）为 一圈内节点的个数A。

因为fast指针是一步走两个节点，slow指针一步走一个节点， 所以 fast指针走过的节点数 = slow指针走过的节点数 * 2：

```
(x + y) * 2 = x + y + n (y + z)
```

两边消掉一个（x+y）: `x + y = n (y + z)`

因为要找环形的入口，那么要求的是x，因为x表示 头结点到 环形入口节点的的距离。

所以要求x ，将x单独放在左面：`x = n (y + z) - y` ,

再从n(y+z)中提出一个 （y+z）来，整理公式之后为如下公式：`x = (n - 1) (y + z) + z` 注意这里n一定是大于等于1的，因为 fast指针至少要多走一圈才能相遇slow指针。

这个公式说明什么呢？

先拿n为1的情况来举例，意味着fast指针在环形里转了一圈之后，就遇到了 slow指针了。

当 n为1的时候，公式就化解为 `x = z`，

这就意味着，**从头结点出发一个指针，从相遇节点 也出发一个指针，这两个指针每次只走一个节点， 那么当这两个指针相遇的时候就是 环形入口的节点**。

也就是在相遇节点处，定义一个指针index1，在头结点处定一个指针index2。

让index1和index2同时移动，每次移动一个节点， 那么他们相遇的地方就是 环形入口的节点。

那么 n如果大于1是什么情况呢，就是fast指针在环形转n圈之后才遇到 slow指针。

其实这种情况和n为1的时候 效果是一样的，一样可以通过这个方法找到 环形的入口节点，只不过，index1 指针在环里 多转了(n-1)圈，然后再遇到index2，相遇点依然是环形的入口节点。

~~~python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        fast = head
        slow = head
        while fast != None and fast.next != None: #当前节点和下一个节点不为空时快指针走两步，慢指针走一步
            #快指针走两步
            fast = fast.next.next 
            #慢指针走一步
            slow = slow.next
            # 相遇的时候离入环节点的距离 == 头节点到入环节点的距离
            if fast == slow:
                #定义两个index，一个从头，一个从相遇点
                index1 = head
                index2 = slow
                #循环两个index每次走1，直到找到相同节点
                while index1 != index2:
                    index1 = index1.next
                    index2 = index2.next
                return index1
        return None
~~~

## 链表总结：

https://programmercarl.com/%E9%93%BE%E8%A1%A8%E6%80%BB%E7%BB%93%E7%AF%87.html#%E9%93%BE%E8%A1%A8%E7%9A%84%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80