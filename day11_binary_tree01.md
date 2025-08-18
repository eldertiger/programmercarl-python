# 二叉树part1

## 一、 递归遍历二叉树
递归遍历主要要想清楚递归的目标，退出的条件，遍历的顺序以及是否需要返回值。

### [144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/description/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        def dfs(node):
            if node is None: return
            res.append(node.val)
            dfs(node.left)
            dfs(node.right)
        dfs(root)
        return res
```

### [145. 二叉树的后序遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/description/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        def dfs(node):
            if node is None: return 
            dfs(node.left)
            dfs(node.right)
            res.append(node.val)
        dfs(root)
        return res
```

### [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/description/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        def dfs(node):
            if node is None: return
            dfs(node.left)
            res.append(node.val)
            dfs(node.right)
        dfs(root)
        return res
```

## 二、 迭代遍历二叉树


### [144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/description/)

```python

class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root: return []
        res = []
        stack = [root]
        while stack:
            node = stack.pop()
            res.append(node.val)
            if node.right: stack.append(node.right)
            if node.left :  stack.append(node.left)
        return res

```

### [145. 二叉树的后序遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/description/)

```python
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root: return []
        res = []
        stack = [root]
        while stack:
            node = stack.pop()
            res.append(node.val)
            if node.left : stack.append(node.left)
            if node.right: stack.append(node.right)

        return res[::-1]
```

### [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/description/)

```python
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root: return []
        res = []
        stack = []
        cur = root
        while cur or stack:
            if cur:
                stack.append(cur)
                cur = cur.left
            else:
                cur = stack.pop()
                res.append(cur.val)
                cur = cur.right
        return res
```

## 三、 统一迭代
### 方法1. 
采用在栈的节点后面添加空指针，用于标记该节点尚未处理，当pop出空节点时，处理下一个栈节点。
### 方法2. 
采用boolean对，为节点添加一个布尔值，表示是否访问过，访问过则下次再遇到就pop到结果中。
### [144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/description/)

```python
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root: return []
        res = []
        stack = [root]
        while stack:
            node = stack.pop()
            if node:
                if node.right: stack.append(node.right)
                if node.left: stack.append(node.left)
                stack.append(node)
                stack.append(None)
            else:
                node = stack.pop()
                res.append(node.val)
        return res
```
```python
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root: return []
        res = []
        stack = [(root, False)]
        while stack:
            node, visited = stack.pop()
            if visited: 
                res.append(node.val)
                continue
            if node.right: stack.append((node.right, False))
            if node.left: stack.append((node.left, False))
            stack.append((node,True))

        return res

```


### [145. 二叉树的后序遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/description/)

```python
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root: return []
        res = []
        stack = [root]
        while stack:
            node = stack.pop()
            if node:
                stack.append(node)
                stack.append(None)
                if node.right: stack.append(node.right)
                if node.left: stack.append(node.left)

            else:
                node = stack.pop()
                res.append(node.val)
        return res
```
```python
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root: return []
        res = []
        stack = [(root,False)]
        while stack:
            node,visited = stack.pop()
            if visited:
                res.append(node.val)
                continue
            stack.append((node,True))
            if node.right: stack.append((node.right,False))
            if node.left: stack.append((node.left,False))
        return res
```

### [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/description/)

```python
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root: return []
        res = []
        stack = [root]
        while stack:
            node = stack.pop()
            if node:
                if node.right: stack.append(node.right)
                stack.append(node)
                stack.append(None)
                if node.left: stack.append(node.left)
            else:
                node = stack.pop()
                res.append(node.val)
        return res
```


```python
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root: return []
        res = []
        stack = [(root, False)]
        while stack:
            node, visited = stack.pop()
            if visited: 
                res.append(node.val)
                continue
            if node.right: stack.append((node.right, False))
            stack.append((node,True))
            if node.left: stack.append((node.left, False))
        return res
```

## 四、二叉树层序遍历
### [102.二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

```python
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root: return []
        res = []
        queue = collections.deque([root])
        while queue:
            level = []
            for _ in range(len(queue)):
                node = queue.popleft()
                level.append(node.val)
                if node.left:  queue.append(node.left)
                if node.right: queue.append(node.right)
            res.append(level)
        return res
```

### [107.二叉树的层次遍历II](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/)
上一道题的逆序即可，注意输出的格式不是数组，而是每层的list。
```python
class Solution:
    def levelOrderBottom(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root: return []
        res = []
        queue = collections.deque([root])
        while queue:
            level = []
            for _ in range(len(queue)):
                node = queue.popleft()
                level.append(node.val)
                if node.left:  queue.append(node.left)
                if node.right: queue.append(node.right)
            res.append(level)
        return res[::-1]
```

### [199.二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/description/)
返回每一层最右边的值即可。
```python
class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        if not root: return []
        res = []
        queue = collections.deque([root])
        while queue:
            level = []
            for _ in range(len(queue)):
                node = queue.popleft()
                level.append(node.val)
                if node.left:  queue.append(node.left)
                if node.right: queue.append(node.right)
            res.append(level[-1])
        return res
```

### [637.二叉树的层平均值](https://leetcode.cn/problems/average-of-levels-in-binary-tree/description/)
```python
class Solution:
    def averageOfLevels(self, root: Optional[TreeNode]) -> List[float]:
        if not root: return []
        res = []
        queue = collections.deque([root])
        while queue:
            level_sum = 0
            count = len(queue)
            for _ in range(count):
                node = queue.popleft()
                level_sum += node.val
                if node.left:  queue.append(node.left)
                if node.right: queue.append(node.right)
            res.append(level_sum/count)
        return res
```


### [429.N叉树的层序遍历](https://leetcode.cn/problems/n-ary-tree-level-order-traversal/description/)
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val: Optional[int] = None, children: Optional[List['Node']] = None):
        self.val = val
        self.children = children
"""

class Solution:
    def levelOrder(self, root: 'Node') -> List[List[int]]:
        if not root: return []
        res = []
        queue = collections.deque([root])
        while queue:
            level = []
            for _ in range(len(queue)):
                node = queue.popleft()
                level.append(node.val)
                if node.children: 
                    for i in range(len(node.children)):
                        queue.append(node.children[i]) 
            res.append(level)
        return res
```

### [515.在每个树行中找最大值](https://leetcode.cn/problems/find-largest-value-in-each-tree-row/)
```python
class Solution:
    def largestValues(self, root: Optional[TreeNode]) -> List[int]:
        if not root: return []
        res = []
        queue = collections.deque([root])
        while queue:
            level_max = -2**31-1
            for _ in range(len(queue)):
                node = queue.popleft()
                level_max = max(level_max, node.val)
                if node.left:  queue.append(node.left)
                if node.right: queue.append(node.right)
            res.append(level_max)
        return res
```

### [116.填充每个节点的下一个右侧节点指针](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/description/)
本体注意需要额外一个指针指向前一个已经遍历过的节点，如果采用指向当前节点下一个node的方法，则判断复杂，并且需要重新将下一个节点入队，得不偿失。
```python
class Solution:
    def connect(self, root: 'Optional[Node]') -> 'Optional[Node]':
        if not root: return root
        queue = collections.deque([root])
        while queue:
            prev = None
            for _ in range(len(queue)):
                node = queue.popleft()
                if prev:
                    prev.next = node
                prev = node
                if node.left:  queue.append(node.left)
                if node.right: queue.append(node.right)
        return root
```
### [117.填充每个节点的下一个右侧节点指针II](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/description/)
和上面一样的代码。。。。
```python
class Solution:
    def connect(self, root: 'Optional[Node]') -> 'Optional[Node]':
        if not root: return root
        queue = collections.deque([root])
        while queue:
            prev = None
            for _ in range(len(queue)):
                node = queue.popleft()
                if prev:
                    prev.next = node
                prev = node
                if node.left:  queue.append(node.left)
                if node.right: queue.append(node.right)
        return root
```
### [104.二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/description/)
记录一下遍历的层数。
```python
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root: return 0
        res = 0
        queue = collections.deque([root])
        while queue:
            for _ in range(len(queue)):
                node = queue.popleft()
                if node.left:  queue.append(node.left)
                if node.right: queue.append(node.right)
            res += 1
        return res
```
### [111.二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/description/)
判断当前层是否有节点为叶子节点，有则返回层数，否则继续层序遍历。
```python
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        if not root: return 0
        res = 0
        queue = collections.deque([root])
        while queue:
            res += 1
            for _ in range(len(queue)):
                node = queue.popleft()
                if not node.left and not node.right:
                    return res
                if node.left:  
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
        return res
```