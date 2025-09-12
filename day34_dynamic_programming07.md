# 动态规划 part7

## [198.打家劫舍](https://leetcode.cn/problems/house-robber/description/)

### 递推思路
当前的选择为偷或者不偷，偷则总金额变为dp[i-2]+nums[i]， 不偷则总金额为dp[i-1]

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if len(nums) == 1: return nums[0]
        dp = [0] * len(nums)
        for i in range(len(nums)):
            dp[i] = max(dp[i-2]+nums[i],dp[i-1])
        return dp[-1]
```

## [213.打家劫舍II](https://leetcode.cn/problems/house-robber-ii/description/)

### 思路
由于是环路，所以要考虑第一个和最后一个房子不能同时被偷，那么可以理解为求第1到倒数第二个房子被间隔偷的最大价值和第二房子到最后一个房子被间隔偷的最大价值的较大值。

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if len(nums)==1: 
            return nums[0]
        dp1 = [0]*len(nums)
        dp2 = [0]*len(nums)
        for i in range(1, len(nums)):
            dp1[i] = max(dp1[i-1],dp1[i-2]+nums[i])
        for i in range(0, len(nums)-1):
            dp2[i] = max(dp2[i-1],dp2[i-2]+nums[i])
        return max(dp1[-1],dp2[-2])
```

## [337.打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/description/)

### 思路

根据当前节点是否偷，返回偷与不偷的最大值：
1. 若偷当前节点，最大值为当前节点和不偷左右子节点的最大值；
2. 若不偷当前节点，最大值为左右子节点偷或不偷的最大值；
3. 若当前节点为null，则返回[0,0]，即没钱。

最后得到根节点的偷与不偷的最大值。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def robtree(self,node):
        if not node: return [0, 0]
        left = self.robtree(node.left)
        right = self.robtree(node.right)
        val1 = node.val + left[0]+right[0] #偷当前节点
        val2 = max(left) + max(right)# 不偷当前节点
        return [val2,val1]
    def rob(self, root: Optional[TreeNode]) -> int:
        dp = self.robtree(root)
        return max(dp[0],dp[1])
        
```
