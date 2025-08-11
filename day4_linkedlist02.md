# 链表 part2
## 一、 [24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/description/)

### 方法：

使用pre，cur双指针分别指向dummyhead和head两个节点，需要调换dummyhead， head， head.next的顺序；  
1. pre和cur分别指向新的cur.next和下一个节点；  
2. 新的pre的下一个节点指向cur，即调换cur和cur.next的方向
3. 重新更新pre和cur的位置。  

注意要满足cur和cur.next都不为空才能交换，否则跳出。  
使用虚拟头节点会方便很多。

```python
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummyhead = ListNode(next=head)
        pre, cur = dummyhead, head
        while cur and cur.next:
            pre.next = cur.next
            cur.next = cur.next.next
            pre.next.next = cur
            pre = cur
            cur = pre.next

        return dummyhead.next
```

## 二、 [19.删除链表的倒数第N个节点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)
### 方法1：
先遍历一遍获得链表长度$length$，然后在寻找目标第$length-n+1$节点前面的一个节点，将其删除即可。时间复杂度$O(n)$

```python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        # find length
        cur = head
        length = 0
        while cur:
            length += 1
            cur = cur.next
        # find target
        dummyhead = ListNode(next = head)
        cur = dummyhead
        for i in range(1, length-n+1): 
            # start from 1 so that the cur will end at the node before the target
            cur = cur.next
        cur.next = cur.next.next
        return dummyhead.next
```

### 方法二： 栈

将所有节点压到栈中，从栈顶pop出n个值，则栈顶对应target之前的那个节点，删除后一个节点即可。  
注意点： 需要将dummyhead也压入栈，才能删除head，否则若要删除head时，$stack[-1]$会报错。

```python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummyhead = ListNode(next = head)
        cur = dummyhead
        stack = []
        while cur:
            stack.append(cur)
            cur = cur.next

        for i in range(n):
            stack.pop()
        prev = stack[-1]
        prev.next = prev.next.next
        return dummyhead.next
```

### 方法三： 双指针
即快指针先走n步，慢指针再走，当快指针到达链表尾部，快慢之间的差距就是n，慢指针对应倒数第n个节点。

```python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummyhead = ListNode(next = head)
        slow, fast = dummyhead,dummyhead
        for _ in range(n):
            fast = fast.next
        while fast.next:
            fast = fast.next
            slow = slow.next
        slow.next = slow.next.next
        return dummyhead.next
```


## 三、[ 面试题 02.07. 链表相交](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/)

### 方法：
A, B链表长度分别为a和b, 交叉段长度为c, 则起始节点到交叉点的距离分别a-c, b-c, 若分别加b和a，则长度相等。即分别遍历一遍自身，再从另一个链表开始遍历，直到二者相等，即为交叉点。

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        A, B = headA, headB
        while A != B:
            A = A.next if A else headB
            B = B.next if B else headA
        return A
```

### 方法二：
A，B链表分别遍历一遍得到长度，然后较长的链表遍历到二者差值长度的节点，同时开始遍历，若相等则输出交叉点。

```python

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        A, B = headA, headB
        lenA, lenB = 0, 0
        while A:
            lenA += 1
            A = A.next
        while B:
            lenB += 1
            B = B.next
        if lenA < lenB:
            headA, headB = headB, headA
            lenA, lenB = lenB, lenA
        A, B = headA, headB
        for _ in range(lenA-lenB):
            A = A.next
        while A:
            if A == B:
                return A
            else:
                A = A.next
                B = B.next
        return 
```



## 四、 [142.环形链表II](https://leetcode.cn/problems/linked-list-cycle-ii/)

### 方法：快慢指针法
slow， fast从head出发，每次分别走一步和两步。若链表存在环形，则二者必定在环内相遇。  
假设环的入口长度为x， 环的长度为y，slow在环内走了z，则存在关系
$2(x+z) = x+ny+z$, fast在环内走了n圈+z后，赶上了slow。   
$x = (n-1)y+(y-z)$  
此时slow重新从head出发，fast从相遇点出发，每次走一步，则二者将在环形入口相遇。无论n为多少，fast剩下的到节点的距离总是和x差整数倍的环数，即二者只会在环的入口相遇。



```python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow, fast = head, head

        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                slow = head
                while slow != fast:
                    fast = fast.next
                    slow = slow.next
                return slow
        return
```