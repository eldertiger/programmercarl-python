# 二叉树part3
## [110.平衡二叉树](https://leetcode.cn/problems/balanced-binary-tree/)
### 方法：后序遍历递归法
后序求高度，左右中；前序求深度，中左右。  
采用求高度的函数，同时判断是否满足平衡二叉树的左右子树高度差小于1的条件。   
1. 节点为None， 则高度为0；
2. 分别获取左右子树高度，任一子树的子树不满足平衡二叉树条件，则返回-1；
3. 对比左右子树高度之差，若大于1，则返回-1；否则返回二者最大值+1(节点本身的高度)
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
            def get_height(node):
                if not node: return 0
                left_height = get_height(node.left)
                if left_height == -1: return -1
                right_height = get_height(node.right)
                if right_height == -1: return -1
                if abs(right_height-left_height)>1: return -1
                return 1+max(left_height, right_height)
            return True if get_height(root)>=0 else False
```

## [257. 二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/)
### 方法： 前序遍历递归法
递归需要传入节点node、保存路径的path和保存结果的res；  
递归的返回条件是到达叶子节点，即左右节点均为空，此时将path保存的路径加入结果中，返回到上一层；  
若左右节点不为空，则分别递归左右节点，注意递归之后要回溯，即path移除栈顶路径。  
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        if not root: return []
        res = []
        path = []
        def traversal(node, path, res):
            path.append(node.val)
            if not node.left and not node.right:
                res.append("->".join(map(str,path)))
                return
            if node.left:
                traversal(node.left, path, res)
                path.pop()
            if node.right:
                traversal(node.right, path, res)
                path.pop()
        traversal(root,path,res)
        return res
```

### 方法2： 前序遍历迭代法
使用sPath存储到当前节点的所有之前遇到的路径，类似于stack的效果。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        if not root: return []
        res = []
        stack = [root]
        sPath = [str(root.val)]
        while stack:
            node = stack.pop()
            path = sPath.pop()
            if not node.left and not node.right:
                res.append(path)
            if node.left:
                stack.append(node.left)
                sPath.append(path+"->"+str(node.left.val))
            if node.right:
                stack.append(node.right)
                sPath.append(path+"->"+str(node.right.val))
        return res
```

## [404.左叶子之和](https://leetcode.cn/problems/sum-of-left-leaves/description/)

### 方法1：迭代法
前序遍历，判断左子树是不是左叶子节点，是则将值加入结果中；否则继续遍历；
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        if not root: return 0
        stack = [root]
        res = 0
        while stack:
            node = stack.pop()
            if node.left and not node.left.left and not node.left.right:
                res += node.left.val
            if node.left:
                stack.append(node.left)
            if node.right:
                stack.append(node.right)
        return res
```

### 方法2： 递归
递归传入参数为当前节点，返回值为当前节点的左叶子节点之和;  
1. 节点为空，返回0；
2. 节点左右子树为空， 返回0；
3. 获得节点左叶子节点之和的值；
4. 若左子树为叶子节点，则返回左叶子节点的值；
5. 获得节点右子树的左叶子节点之和值；
6. 返回节点左右子树之和；

```python
class Solution:
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        if not root: return 0
        if not root.left and not root.right: return 0

        left_values = self.sumOfLeftLeaves(root.left)
        if root.left and not root.left.left and not root.left.right:
            left_values = root.left.val
        right_values = self.sumOfLeftLeaves(root.right)
        return left_values+right_values
```

## [222. 完全二叉树的节点个数](https://leetcode.cn/problems/count-complete-tree-nodes/description/)

### 方法1：迭代遍历法
没啥好说的，遍历即可， 但是时间复杂度为$O(n)$
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root: return 0
        stack = [root]
        res = 0
        while stack:
            node = stack.pop()
            res += 1
            if node.left: stack.append(node.left)
            if node.right: stack.append(node.right)
        return res
```
### 方法2：递归遍历
```python
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root: return 0
        return 1 + self.countNodes(root.left)+self.countNodes(root.right)
```
### 方法3：利用完全二叉树的特性
如果当前节点的最左叶子节点和最右叶子节点深度相同，则说明当前节点为满二叉树，其节点数量为2**depth-1；  
若深度不同，则分别求左右子树的节点数量，还是利用上述方法递归。
理想的情况，完全满二叉树，只需要遍历$O(log n)$次，最差情况$O(logn*logn)$
```python
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root: return 0
        left = root.left
        right = root.right
        count = 1
        while left and right:
            count+=1
            left = left.left
            right = right.right
        if not left and not right:
            return 2**count - 1
        return 1 + self.countNodes(root.left)+self.countNodes(root.right)
```