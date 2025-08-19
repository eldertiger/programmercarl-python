# 二叉树part2

## [226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/description/)
### 方法1：递归前序遍历
交换所有的左右节点即翻转二叉树。
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root: return root
        root.left, root.right = root.right, root.left
        self.invertTree(root.left)
        self.invertTree(root.right)
        return root
```

### 方法2： 迭代前序遍历
使用栈
```python
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root: return root
        stack = [root]
        while stack:
            node = stack.pop()
            node.left, node.right = node.right, node.left
            if node.left: stack.append(node.left)
            if node.right: stack.append(node.right)
        return root
```
### 方法3： 迭代前序遍历
使用队列
```python
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root: return root
        queue = collections.deque([root])
        while queue:
            node = queue.popleft()
            node.left, node.right = node.right, node.left
            if node.left: queue.append(node.left)
            if node.right: queue.append(node.right)
        return root
```

## [101. 对称二叉树](http://leetcode.cn/problems/symmetric-tree/description/)

### 方法：递归法

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        def compare(left, right):
            if left is None and right is None: return True
            elif left is None: return False
            elif right is None: return False
            elif left.val != right.val: return False
            else: 
                outside = compare(left.left, right.right)
                inside = compare(left.right, right.left)
                return outside and inside
        if not root: return True
        return compare(root.left, root.right)
```

### 方法： 迭代法
使用队列实现

```python
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        if not root: return True
        queue = collections.deque()
        queue.append(root.left)
        queue.append(root.right)
        while queue:
            leftnode = queue.popleft()
            rightnode = queue.popleft()
            if not leftnode and not rightnode:
                continue
            if not leftnode or not rightnode or leftnode.val != rightnode.val:
                return False
            queue.append(leftnode.left)
            queue.append(rightnode.right)
            queue.append(leftnode.right)
            queue.append(rightnode.left)
        return True
```

### 方法3： 迭代法
使用栈
```python
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        if not root: return True
        stack = []
        stack.append(root.right)
        stack.append(root.left)
        while stack:
            leftnode = stack.pop()
            rightnode = stack.pop()
            if not leftnode and not rightnode:
                continue
            if not leftnode or not rightnode or leftnode.val != rightnode.val:
                return False
            stack.append(rightnode.left)
            stack.append(leftnode.right)
            stack.append(rightnode.right)
            stack.append(leftnode.left)
        return True
```

## [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/description/)
当前节点高度为1，叠加最大的左右子节点深度，即为最大深度。
### 方法1：后序递归法
```python
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        def get_depth(node):
            if not node: return 0
            leftdepth = get_depth(node.left)
            rightdepth = get_depth(node.right)
            depth = 1 + max(leftdepth,rightdepth)
            return depth
        return get_depth(root)
```
### 方法2：层序遍历
遍历所有层，最后一层即为最大深度。
```python
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root: return 0
        queue = collections.deque([root])
        depth = 0
        while queue:
            depth += 1
            for _ in range(len(queue)):
                node = queue.popleft()
                if node.left: queue.append(node.left)
                if node.right: queue.append(node.right)
        return depth
```

## [111. 二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/description/)

### 方法1： 层序遍历
第一个左右节点均为空时对应的节点深度即为最小深度。
```python
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        if not root: return 0
        queue  = collections.deque([root])
        res = 0
        while queue:
            res += 1
            for _ in range(len(queue)):
                node = queue.popleft()
                if not node.right and not node.left:
                    return res
                if node.left: queue.append(node.left)
                if node.right: queue.append(node.right)
        return res
```
### 方法2：递归法
左节点为空时，最小深度为当前节点高度1加上右节点的深度，右节点为空，同理。
左右节点均不为空时，返回二者的深度最小值+1。  

```python
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        def get_depth(node):
            if not node: return 0
            leftdepth = get_depth(node.left)
            rightdepth = get_depth(node.right)
            if not node.left:
                return 1 + rightdepth
            if not node.right:
                return 1 + leftdepth
            return  1 + min(leftdepth, rightdepth)
        return  get_depth(root)
```

## [559. N 叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-n-ary-tree/description/)

### 方法1： 递归
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val: Optional[int] = None, children: Optional[List['Node']] = None):
        self.val = val
        self.children = children
"""

class Solution:
    def maxDepth(self, root: 'Node') -> int:
        def get_depth(node):
            if not node: return 0
            depth = 0
            for child in node.children:
                depth = max(depth, get_depth(child))
            return 1 + depth
        return get_depth(root)
```

### 方法2： 迭代
```python
class Solution:
    def maxDepth(self, root: 'Node') -> int:
        if not root: return 0
        queue = collections.deque([root])
        depth = 0
        while queue:
            depth += 1
            for _ in range(len(queue)):
                node = queue.popleft()
                for child in node.children:
                    queue.append(child) 
        return depth
```
