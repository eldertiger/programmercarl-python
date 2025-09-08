# 动态规划 part2
## [62.不同路径](https://leetcode.cn/problems/unique-paths/description/)

### 思路
最上面一行和最左侧一列都只有一种路径，从1开始遍历m、n，当前路径数量等于左边和上方的路径之和。

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [[0]*n for _ in range(m)]
        for j in range(n):
            dp[0][j] = 1
        for i in range(m):
            dp[i][0] = 1 
        for i in range(1,m):
            for j in range(1,n):
                dp[i][j] = dp[i-1][j] + dp[i][j-1]
        return dp[m-1][n-1]
```

## [63. 不同路径 II](https://leetcode.cn/problems/unique-paths-ii/description/)

### 思路
在上一题的基础上加上判断当前dp格点是否存在障碍物，是的话则dp归零。  
初始化最左列和最上面一行则障碍物和之后都是0。

```python
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])
        dp = [[0]*n for _ in range(m)]
        for i in range(m):
            if obstacleGrid[i][0] != 1:
                dp[i][0] = 1
            else:
                break
        for j in range(n):
            if obstacleGrid[0][j] != 1:
                dp[0][j] = 1
            else:
                break
        for i in range(1, m):
            for j in range(1, n):
                if obstacleGrid[i][j] != 1:
                    dp[i][j] = dp[i][j -1] + dp[i - 1][j]
                else:
                    dp[i][j] = 0
        return dp[m-1][n-1]
```

## [343. 整数拆分](https://leetcode.cn/problems/integer-break/description/)

### 思路
当前整数的拆分乘积等于j * (i - j) 直接相乘和j * dp[i - j]的最大值，每次遍历1到i//2，可得到当前整数的最大拆分乘积；

```python
class Solution:
    def integerBreak(self, n: int) -> int:
        dp = [0] * (n+1)
        dp[2] = 1
        for i in range(3,n+1):
            for j in range(1,i//2+1):
                dp[i] = max(dp[i], max((i-j)*j,dp[i-j]*j))
        return dp[n]
```

### 贪心
显然尽可能拆分成3，是收益最大的，因为$3*3>2*2*2$;  
注意2和3都要拆分。

```python
class Solution:
    def integerBreak(self, n: int) -> int:
        res = 1
        if n == 2: return 1
        if n == 3: return 2
        if n == 4: return 4
        while n > 4:
            n -= 3
            res *= 3
        return res*n
```

## [96.不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/description/)

### 思路
递归公式：
当前二叉搜索树的数量等于所有元素分别为头节点时剩下元素的二叉搜索树数量相加
然后就很容易得到如下代码

```python
class Solution:
    def numTrees(self, n: int) -> int:
        dp = [0]*(n+1)
        dp[0] = 1
        for i in range(1,n+1):
            for j in range(1,i+1):
                dp[i] += dp[i-j]*dp[j-1]
        return dp[n]
```