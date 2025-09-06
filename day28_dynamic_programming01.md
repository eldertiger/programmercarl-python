# 动态规划 part1

## [509. 斐波那契数](https://leetcode.cn/problems/fibonacci-number/)
### 思路
没啥好说的，只需要记录目标的两个数值，交替计算即可；
时间复杂度O(n);  
空间复杂度O(1)。  

```python
class Solution:
    def fib(self, n: int) -> int:
        q = 0
        p = 1
        for i in range(n):
            p, q = p+q, p
        return q
```

## [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

### 思路 
当前楼梯有两种到达方式，从倒数第二级楼梯或者倒数第一级楼梯爬上来，于是递归公式为：
$dp[n] = dp[n-1] + dp[n-2]$

递归初始值,不需要考虑dp[0]：

$dp[1] = 1$

$dp[2] = 2$

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        dp = [1,2]
        for i in range(n-1):
            dp[1], dp[0] = dp[0] + dp[1], dp[1]
        return dp[1]
```

## [746. 使用最小花费爬楼梯](https://leetcode.cn/problems/min-cost-climbing-stairs/description/)

## 思路
递归公式为：  
$dp[n] = min(dp[n-1]+cost[n-1],dp[n-2]+cost[n-2])$

初始值,不需要花钱：  
$dp[0] = 0$  
$dp[1] = 0$

```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        dp = [0, 0]
        for i in range(2, len(cost)+1):
            dp[0], dp[1] = dp[1], min(dp[0]+cost[i-2],dp[1]+cost[i-1])
        return dp[1]
```