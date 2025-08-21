# 二叉树part4
## [513. 找树左下角的值](https://leetcode.cn/problems/find-bottom-left-tree-value/description/)
###  方法： 层序遍历
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        if not root: return []
        queue = collections.deque([root])
        while queue:
            for i in range(len(queue)):
                node = queue.popleft()
                if i == 0:
                  res = node.val
                if node.left: queue.append(node.left)
                if node.right: queue.append(node.right)
        return res
```

### 方法2： 递归
递归传入的参数为当前节点，需要获得当前节点的深度和值，如果深度超过当前最大深度，则赋值给最大深度和结果；  
最左叶子节点实际上就是前序遍历时，第一个最大深度的叶子节点。  
注意外层为函数时，内层只能用nonlocal去声明外层函数中的变量，否则使用global声明为全局变量。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        res = 0
        max_depth = 0
        def dfs(node, depth):
            if not node: return
            depth += 1
            nonlocal max_depth, res
            if not node.left and not node.right:

                if max_depth < depth:
                    max_depth = depth
                    res = node.val
                    return
            if node.left:
                dfs(node.left, depth)
            if node.right:
                dfs(node.right, depth)

        dfs(root, 0)
        return res            
```

## [112. 路径总和](https://leetcode.cn/problems/path-sum/)
### 方法：递归法
递归传入参数为当前节点和剩余目标值。
1. 若当前节点剩余目标值为0， 则找到目标路径总和，返回True；
2. 递归左节点，递归右节点，注意减去左右子节点的值，返回时重新加回左右子节点的值；

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        def dfs(node, targetSum):
            if not node.left and not node.right:
                if targetSum == 0:
                    return True
            if node.left:
                targetSum -= node.left.val
                if dfs(node.left, targetSum):
                    return True
                targetSum += node.left.val
            if node.right:
                targetSum -= node.right.val
                if dfs(node.right,targetSum):
                    return True
                targetSum += node.right.val
            return False
        if not root: return False
        return dfs(root, targetSum-root.val)
    
```

### 方法2：迭代法
迭代法使用(节点, 累计和)的pair压入栈，直到找到满足累计和等于目标值的叶子节点，否则继续遍历左右子节点。
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root: return False
        stack = [(root, root.val)]
        while stack:
            node, node_sum = stack.pop()
            if not node.left and not node.right and node_sum == targetSum:
                return True
            if node.left:
                stack.append((node.left, node_sum+node.left.val))
            if node.right:
                stack.append((node.right, node_sum+node.right.val))
        return False
```

## [113. 路径总和 II](https://leetcode.cn/problems/path-sum-ii/description/)
### 方法：
注意传递到res的path应该是其中的值，而不是path本身的引用，如果使用`res.append(path)`，最终结果将只包含根节点。
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        if not root: return []
        res = []
        path = [root.val]
        def dfs(node, targetSum):
            if not node.left and not node.right:
                if targetSum == 0:
                    res.append(path[:])
            if node.left:
                targetSum -= node.left.val
                path.append(node.left.val)
                dfs(node.left, targetSum)
                path.pop()
                targetSum += node.left.val
            if node.right:
                targetSum -= node.right.val
                path.append(node.right.val)
                dfs(node.right,targetSum)
                path.pop()
                targetSum += node.right.val

        dfs(root, targetSum-root.val)
        return res
```

简化版本:
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        res = []
        path = []
        def dfs(node, targetSum):
            if not node: return
            path.append(node.val)
            targetSum -= node.val
            if not node.left and not node.right:
                if targetSum == 0:
                    res.append(path[:])
            dfs(node.left, targetSum)
            dfs(node.right, targetSum)
            path.pop()

        dfs(root, targetSum)
        return res
```

## [106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)
必须掌握的二叉树重构。
### 方法：
后序遍历的最后一个值是根节点，根据根节点，可以将中序遍历划分为左子树和右子树，根据中序遍历的左右子树长度可以划分后序遍历的左右子树；  
分别继续遍历左右子树。
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> Optional[TreeNode]:
        if not inorder or not postorder: 
            return None
        root_val = postorder[-1]
        root = TreeNode(root_val)
        for i in range(len(inorder)):
            if inorder[i] == root_val:
                seperate_index = i
        inorder_left = inorder[:seperate_index]
        inorder_right = inorder[seperate_index+1:]
        postorder_left = postorder[:len(inorder_left)]
        postorder_right = postorder[len(inorder_left):-1]
        root.left = self.buildTree(inorder_left, postorder_left)
        root.right = self.buildTree(inorder_right, postorder_right)
        return root
```

## [105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/)
必须掌握的二叉树重构。
### 方法：
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if not preorder or not inorder: return None
        root_val = preorder[0]
        root = TreeNode(root_val)
        for i in range(len(inorder)):
            if inorder[i] == root_val:
                seperate_index = i
        inorder_left = inorder[:seperate_index]
        inorder_right = inorder[seperate_index+1:]
        preorder_left = preorder[1:1+len(inorder_left)]
        preorder_right = preorder[1+len(inorder_left):]
        root.left = self.buildTree(preorder_left,inorder_left)
        root.right = self.buildTree(preorder_right,inorder_right)
        return root
```