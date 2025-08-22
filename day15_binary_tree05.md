# 二叉树part5
## [654. 最大二叉树](https://leetcode.cn/problems/maximum-binary-tree/description/)
### 方法：递归
找到最大值作为root节点，左右分别递归。使用切片法。
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> Optional[TreeNode]:
        if not nums: return None
        max_val = nums[0]
        max_ind = 0
        for i in range(len(nums)):
            if  nums[i] > max_val:
                max_val = nums[i]
                max_ind = i
        root = TreeNode(max_val)
        root.left = self.constructMaximumBinaryTree(nums[:max_ind])
        root.right = self.constructMaximumBinaryTree(nums[max_ind+1:])
        return root
```
简化使用左右指针：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> Optional[TreeNode]:
        def traversal(left, right):
            if left >= right: return None
            max_val = nums[left]
            max_ind = left
            for i in range(left,right):
                if  nums[i] > max_val:
                    max_val = nums[i]
                    max_ind = i
            node = TreeNode(max_val)
            node.left = traversal(left,max_ind)
            node.right = traversal(max_ind+1,right)
            return node
        root = traversal(0,len(nums))
        return root
```

## [617.合并二叉树](https://leetcode.cn/problems/merge-two-binary-trees/description/)
### 方法1：递归
每个节点判断是否存在，都存在则值为二者之和，否则返回另一个存在的节点；
继续递归左右子节点。
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root1: return root2
        if not root2: return root1
        root = TreeNode(root1.val+root2.val)
        root.left = self.mergeTrees(root1.left,root2.left)
        root.right = self.mergeTrees(root1.right,root2.right)
        return root
```
### 方法2：迭代
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root1: return root2
        if not root2: return root1
        queue = collections.deque()
        queue.append(root1)
        queue.append(root2) 
        while queue:
            node1 = queue.popleft()
            node2 = queue.popleft()
            node1.val += node2.val
            if node1.left and node2.left:
                queue.append(node1.left)
                queue.append(node2.left)
            if node1.right and node2.right:
                queue.append(node1.right)
                queue.append(node2.right)
            if not node1.left and node2.left:
                node1.left = node2.left
            if not node1.right and node2.right:
                node1.right = node2.right
        return root1
```

## [700. 二叉搜索树中的搜索](https://leetcode.cn/problems/search-in-a-binary-search-tree/description/)
### 方法1： 递归
二叉搜索树其左子树均小于父节点，右子树均大于父节点，分为三种情况讨论下，返回节点或左右子树递归即可。
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root or root.val == val: return root
        if root.val < val: return self.searchBST(root.right,val)
        if root.val > val: return self.searchBST(root.left, val)
```

### 方法2： 迭代
```python
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        while root:
            if root.val > val: root = root.left
            elif root.val < val: root = root.right
            else: 
                break
        return root
```
## [98.验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)
### 方法1： 递归
二叉搜索树的中序遍历元素是严格递增的。
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        self.max_val = float(-inf)
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        if not root: return True
        left = self.isValidBST(root.left)
        if self.max_val<root.val:
            self.max_val = root.val
        else:
            return False
        right = self.isValidBST(root.right)
        return left and right
```
### 方法2： 双指针
中序遍历使用两个指针分别指向前后两个节点。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        stack = []
        cur = root
        pre = None
        while cur or len(stack) > 0:
            if cur:
                stack.append(cur)
                cur = cur.left 
            else:
                cur = stack.pop()
                if pre and cur.val <= pre.val:
                    return False
                pre = cur
                cur = cur.right
        return True
```

