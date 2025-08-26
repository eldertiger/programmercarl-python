# 二叉树 part8

## [669. 修剪二叉搜索树](https://leetcode.cn/problems/trim-a-binary-search-tree/)

### 方法： 递归法

递归传入参数为当前节点和上下边界，返回修剪后的当前节点。

```python

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def trimBST(self, root: Optional[TreeNode], low: int, high: int) -> Optional[TreeNode]:
        if not root: return None
        if root.val < low:
            return self.trimBST(root.right, low, high)
        if root.val > high:
            return self.trimBST(root.left, low, high)
        root.left  = self.trimBST(root.left, low, high)
        root.right = self.trimBST(root.right,low, high)
        return root
```

### 方法2： 迭代法

先处理根节点，根节点大于或小于边界时，直接剪枝另外一边的子树，
在剪枝的时候，可以分为三步：

1. 将root移动到[L, R] 范围内，注意是左闭右闭区间，
2. 剪枝左子树
3. 剪枝右子树

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def trimBST(self, root: Optional[TreeNode], low: int, high: int) -> Optional[TreeNode]:
        if not root: return None
        while root and (root.val < low or root.val > high):
            if root.val < low: root = root.right
            else: root = root.left
        cur = root
        while cur:
            while cur.left and cur.left.val < low:
                cur.left = cur.left.right
            cur = cur.left
        cur = root
        while cur:
            while cur.right and cur.right.val > high:
                cur.right = cur.right.left
            cur = cur.right
        return root
```

## [108. 将有序数组转换为二叉搜索树](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/description/)

### 方法： 递归法

递归传入的参数为原始数组和左右边界；  
父节点为数组正中间的值，左右子节点分别是左右两边的数组返回的父节点。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        def traversal(nums, left, right):
            if left > right: return None
            mid = left + (right-left)//2
            root = TreeNode(nums[mid])
            root.left = traversal(nums, left, mid-1)
            root.right = traversal(nums, mid+1, right)
            return root
        return traversal(nums,0, len(nums)-1)
```

精简如下：

```python
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        if not nums: return None
        mid = len(nums)//2
        root = TreeNode(nums[mid])
        root.left = self.sortedArrayToBST(nums[:mid])
        root.right = self.sortedArrayToBST(nums[mid+1:])
        return root
```

## [538.把二叉搜索树转换为累加树](https://leetcode.cn/problems/convert-bst-to-greater-tree/)

### 方法：递归法

二叉搜索树的中序遍历为单调递增数组，只需要逆向右中左遍历，累加值即可；

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def convertBST(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        cumSum = 0
        def traversal(node):
            if not node: return
            traversal(node.right)
            nonlocal cumSum
            node.val += cumSum
            cumSum = node.val
            traversal(node.left)
        traversal(root)
        return root
```

或者：

```python
class Solution:
    def __init__(self):
        self.count = 0
    def convertBST(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root: return
        self.convertBST(root.right)
        root.val += self.count
        self.count = root.val
        self.convertBST(root.left)
        return root

```

### 方法：迭代法

```python
class Solution:
    def convertBST(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root: return 0
        stack = []
        cur = root
        pre = None
        cumSum = 0
        while cur or stack:
            if cur:
                stack.append(cur)
                cur = cur.right
            else:
                cur = stack.pop()
                cur.val += cumSum
                cumSum = cur.val
                cur = cur.left
        return root

```