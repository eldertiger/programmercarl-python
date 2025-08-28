# 链表 part1

## 一、[203.移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/description/)

>给你一个链表的头节点 head 和一个整数 val ，请你删除链表中所有满足 Node.val == val 的节点，并返回 新的头节点 。
 

### 方法：
新建一个dummyhead指向head，从dummyhead开始每次判断下一个节点的值是否为val，是则next跳过该节点，否则移动cur到该节点。
1. 时间复杂度： $O(n)$
2. 空间复杂度: $O(1)$


```python
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        dummyhead = ListNode(next=head)
        cur = dummyhead
        while cur.next:
            if cur.next.val==val:
                cur.next = cur.next.next
            else:
                cur = cur.next
        return dummyhead.next
```

若不使用dummyhead：  
需要最后对head进行一次判断。
```python
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        if not head: return
        cur = head
        while cur.next:
            if cur.next.val == val:
                cur.next = cur.next.next
            else:
                cur = cur.next
        return head if head.val != val else head.next
```

## 二、 [707. 设计链表](https://leetcode.cn/problems/design-linked-list/description/)

### 方法:
链表长度为1，head对应的index为0, 若index为1，则无法获取head.next的值；同理，无法删除，但是可以添加。

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class MyLinkedList:

    def __init__(self):
        self.dummyhead = ListNode() # 初始化dummy节点
        self.size = 0               # 初始化链表长度

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size: # 注意等于号
            return -1
        current = self.dummyhead
        for i in range(index+1):
            current = current.next
        return current.val

    def addAtHead(self, val: int) -> None:
        self.dummyhead.next = ListNode(val=val,next=self.dummyhead.next)
        self.size += 1

    def addAtTail(self, val: int) -> None:
        current = self.dummyhead
        while current.next: # 最终指向尾部节点
            current = current.next
        current.next= ListNode(val = val)
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.size: # # 注意没有等于号
            return -1
        current = self.dummyhead
        for i in range(index):# 最终指向index节点上一个节点
            current = current.next
        current.next = ListNode(val = val,next = current.next)
        self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size: # 注意等于号
            return -1
        current = self.dummyhead
        for i in range(index):
            current = current.next
        current.next = current.next.next 
        self.size -= 1
```

## 三、 [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/description/)

> 给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。

# 方法：
双指针将链表方向切换。
pre需要为None， 每次先预存cur.next, 然后调换cur.next的方向，移动pre，移动cur。返回最后一个节点，即pre，因为此时cur已经移动到tail.next了。

```python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        pre = None
        cur = head
        while cur:
            tmp = cur.next
            cur.next = pre
            pre = cur
            cur = tmp
        return pre
```
