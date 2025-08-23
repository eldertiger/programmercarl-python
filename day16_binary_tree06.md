# 二叉树part6
## [530.二叉搜索树的最小绝对差](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)

### 方法1： 递归法
二叉搜索树中序遍历是有序递增数组，所以中序遍历一遍相邻节点的差值就可以了。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        pre = None
        min_val = float(inf)
        def traversal(cur):
            if not cur: return
            traversal(cur.left)
            nonlocal pre, min_val
            if pre:
                min_val = min(min_val, cur.val - pre.val)
            pre = cur
            traversal(cur.right)
        traversal(root)
```

### 方法2： 迭代法

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        if not root: return
        stack = []
        pre = None
        cur = root
        min_val = float(inf)
        while cur or stack:
            if cur:
                stack.append(cur)
                cur = cur.left
            else:
                cur = stack.pop()
                if pre:
                    min_val = min(min_val, cur.val - pre.val)
                pre = cur
                cur = cur.right
        return min_val
```

## [501.二叉搜索树中的众数](https://leetcode.cn/problems/find-mode-in-binary-search-tree/)


### 方法1：迭代法
注意pre初始化的赋值，以及众数的更新，计数相等则添加，计数超过则重新初始化为当前节点值。
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        if not root: return
        stack = []
        pre = None
        cur = root
        while cur or stack:
            if cur:
                stack.append(cur)
                cur = cur.left
            else:
                cur = stack.pop()
                if pre:
                    if cur.val == pre.val:
                        mode_count += 1
                        if mode_count == mode_max:
                            mode_val.append(cur.val)
                        elif mode_count > mode_max:
                            mode_max = mode_count
                            mode_val = [cur.val]
                    else:
                        mode_count = 1
                        if mode_count == mode_max:
                            mode_max = mode_count
                            mode_val.append(cur.val)
                else:
                    mode_val = [cur.val]
                    mode_count = 1
                    mode_max = 1
                pre = cur
                cur = cur.right
        return mode_val
```

### 方法2： 递归法
类似于迭代法，
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        self.pre = None
        self.mode_val = []
        self.mode_max = 0
        self.mode_count = 0

    def traversal(self, cur):
        if not cur: return
        self.traversal(cur.left)
        if self.pre:
            if self.pre.val == cur.val:
                self.mode_count += 1
                if self.mode_count == self.mode_max:
                    self.mode_val.append(cur.val)
                elif self.mode_count > self.mode_max:
                    self.mode_max = self.mode_count
                    self.mode_val = [cur.val]
            else:
                self.mode_count = 1
        else:
            self.mode_count = 1
            self.mode_max = 1
            self.mode_val.append(cur.val)
        self.pre = cur
        self.traversal(cur.right)
    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        self.traversal(root)
        return self.mode_val
```

## [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

### 方法：递归法
本题要从下往上遍历，需要使用后序遍历(左右中)
递归法传入参数为当前节点，p和q，如果遇到p和q则返回p和q。  
最近的公共祖先的左右子树均返回值，表示其为p和q的最近公共祖先，向上返回该公共祖先。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root or root == p or root == q: return root
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        if left and right: return root
        if left: return left
        if right: return right
        return None
```
