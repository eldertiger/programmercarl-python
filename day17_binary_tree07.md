# 二叉树part7
## [235. 二叉搜索树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/)
对于二叉搜索树，公共祖先的值应该在两个目标值之间。从上到下遍历时，遇到的第一个值在目标值之间的节点即为最近公共祖先。

### 方法： 递归法

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if root.val < q.val and root.val < p.val:   
            return self.lowestCommonAncestor(root.right, p, q)
        elif root.val > q.val and root.val > p.val:
            return self.lowestCommonAncestor(root.left, p, q)
        else:
            return root
```
### 方法： 迭代法

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        queue = collections.deque([root])
        while queue:
            node = queue.popleft()
            if node.val > p.val and node.val > q.val:
                queue.append(node.left)
            elif node.val < p.val and node.val < q.val:
                queue.append(node.right)
            else:
                return node
```
精简如下：
```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        while root:
            if root.val > p.val and root.val > q.val:
                root = root.left
            elif root.val < p.val and root.val < q.val:
                root =root.right
            else:
                return root
```

## [701.二叉搜索树中的插入操作](https://leetcode.cn/problems/insert-into-a-binary-search-tree/description/)

二叉搜索树插入一个值，不要求平衡，则可以插到叶子节点上。
### 方法：迭代法

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root: return TreeNode(val)
        cur = root
        while cur:
            if cur.val > val: 
                if cur.left: 
                    cur = cur.left
                else:
                    cur.left = TreeNode(val)
                    return root
            else:
                if cur.right:
                    cur = cur.right
                else:
                    cur.right = TreeNode(val)
                    return root
```

### 方法： 递归法

```python
class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root: root =  TreeNode(val)
        if root.val > val: root.left =  self.insertIntoBST(root.left, val)
        if root.val < val: root.right =  self.insertIntoBST(root.right, val)
        return root
```

## [450.删除二叉搜索树中的节点](https://leetcode.cn/problems/delete-node-in-a-bst/)
分类讨论：
1. 若删除目标不在二叉树，则返回原本二叉树；
2. 若删除节点为叶子，则直接删除；
3. 若删除节点左右有一空，则让不空的子节点继承自己的位置即可；
4. 若删除节点左右均不为空，则可以选择让左子树或右子树继承位置，同时让另外一边的子树放到右子树的最左叶子节点或左子树的最右叶子节点，满足二叉搜索树的条件。

### 方法：递归法
终止条件： 
1. 没有找到目标值，返回None；
2. 等于root值，则分类讨论左右节点；
3. 若root值小于目标值，则返回右子树的删除结果；
4. 若root值大于目标值，则返回左子树的删除结果；

```python
class Solution:
    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        if not root: return None
        if root.val == key:
            if not root.left and not root.right:
                return None
            elif not root.left:
                return root.right
            elif not root.right:
                return root.left
            else:
                cur = root.right
                while cur.left:
                    cur = cur.left
                cur.left = root.left
                return root.right
        elif root.val > key:
            root.left = self.deleteNode(root.left, key)
        elif root.val < key:
            root.right = self.deleteNode(root.right, key)
        return root

```


