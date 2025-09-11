# 动态规划 part6

## [322. 零钱兑换](https://leetcode.cn/problems/coin-change/description/)

### 思路
每次更新dp[j]需要dp[j]和dp[j-coins]+1的数量比较选出较小值，故而初始化需要用最大的正数来避免选不了较小值。  
而dp[0] = 0,因为这里是个数，不是组合数。

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [float('inf')]*(amount+1)
        dp[0] = 0
        for i in range(len(coins)):
            for j in range(coins[i], amount+1):
                dp[j] = min(dp[j],dp[j-coins[i]]+1)
        if dp[-1] != float('inf'):
            return dp[-1]
        else:
            return -1
```

##