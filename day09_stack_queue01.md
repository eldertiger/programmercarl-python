# 栈和队列part1

## 一、[232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)

### 方法1：
使用两个栈(python用列表)来模拟队列，一个入队，一个出队。  
入队直接进入A栈；  
需要peek时，从B栈读出最上面的值即可，需要将A栈依次压到B栈中。判断如下：  
1. 如果B栈不空，直接读取最上面的值。
2. 如果B栈空且A栈空，则无法peek，返回-1；
3. 如果B栈空而A栈不空，则触发一次A栈全部压到B栈的操作，再读取最上面的值。

pop操作就是先peek，然后触发一次B出栈，返回peek的值。  

1. 时间复杂度$O(1)$，出栈入栈查询均为$O(1)$操作;
2. 空间复杂度$O(n)$
```python
class MyQueue:

    def __init__(self):
        self.A = []
        self.B = []

    def push(self, x: int) -> None:
        self.A.append(x)        

    def pop(self) -> int:
        peek = self.peek()
        self.B.pop()
        return peek
        
    def peek(self) -> int:
        if self.B: return self.B[-1]
        if not self.A: return -1
        while self.A:
            self.B.append(self.A.pop())
        return self.B[-1]

    def empty(self) -> bool:
        if not self.A and not self.B:
            return True
        else:
            return False
```

### 方法2：
也可以先pop，再append的方法实现peek，就和上面反过来就行了。
```python
class MyQueue:

    def __init__(self):
        self.A = []
        self.B = []

    def push(self, x: int) -> None:
        self.A.append(x)        

    def pop(self) -> int:
        if self.empty(): return -1
        if self.B:
            return self.B.pop()
        else:
            while self.A:
                self.B.append(self.A.pop())
            return self.B.pop()
        
    def peek(self) -> int:
        pop = self.pop()
        self.B.append(pop)
        return pop

    def empty(self) -> bool:
        return not (self.A or self.B)
```

## 二、[225. 用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/)

### 方法：
使用两个队列来模拟栈，栈的特性是后入先出，队列是先入先出，那么为了用队列实现栈，就得让后入的元素排到队列前面，需要将所有原来的元素出栈，然后再入栈。  
我们用一个队列保留所有原来的元素(满足后入先出)，用另外一个队列入栈，然后将原有元素加到该队列后面，再交换队列，实现入栈。  
出栈就是保留元素的队列出队即可(后入元素在队列首位)。 
1. 入栈时间复杂度$O(n)$，出栈$O(1)$
2. 空间复杂度$O(n)$
```python
from collections import deque
class MyStack:

    def __init__(self):
        self.A = deque()
        self.B = deque()

    def push(self, x: int) -> None:
        self.A.append(x)
        while self.B:
            self.A.append(self.B.popleft())
        self.A, self.B = self.B, self.A

    def pop(self) -> int:
        return self.B.popleft()

    def top(self) -> int:
        return self.B[0]

    def empty(self) -> bool:
        return not (self.A or self.B)
```

## 三、 [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)
### 方法：


使用字典存储左右括号配对，用于匹配出栈；  
有效括号满足左括号入栈，遇到右括号时将对应栈顶左括号出栈，最终栈长度为0； 
注意非左括号，且栈为空或者栈的最后一个括号不是对应右括号，则不满足有效括号；否则可以出栈。

1. 时间复杂度$O(n)$
2. 空间复杂度$O(n)$, 栈占用线性空间大小，字典(哈希表)占用常数大小。
```python
class Solution:
    def isValid(self, s: str) -> bool:
        brackets = { '(':')','{':'}','[':']' }
        stack = []
        for i in range(len(s)):
            c = s[i]
            if c in brackets : 
                stack.append(c)
            elif not stack or brackets[stack[-1]] != c:
                return False
            else:# brackets[stack[-1]] == c
                stack.pop()
        return not stack
```

## 四、[1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/description/)
### 方法：
本题和上一题有效的括号类似，判断栈顶元素与入栈元素是否相同，相同则出栈，到下一个元素。不同则入栈。
```python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        stack = []
        for c in s:
            if not stack or c != stack[-1]:
                stack.append(c)
            else:#  c == stack[-1]
                stack.pop()
        return  ''.join(stack)
```