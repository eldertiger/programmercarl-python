# 动态规划 paart9
## [188. 买卖股票的最佳时机 IV](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/description/)

### 思路
从上一题的两次到k次，就是重复多次么，递推公式还是一样的，初始化做好，循环写好就可以了
```python
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        dp = [[0]*(2*k+1) for _ in range(len(prices))]
        for j in range(0,2*k,2):
            dp[0][j+1] = -prices[0]
        
        for i in range(1,len(prices)):
            for j in range(0,2*k,2):
                dp[i][j+1] = max(dp[i-1][j+1],dp[i-1][j]-prices[i])
                dp[i][j+2] = max(dp[i-1][j+2],dp[i-1][j+1]+prices[i])

        return dp[-1][-1] if dp[-1][-1] > 0 else 0
```

#### j循环到k:

```python
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        dp = [[0]*(2*k+1) for _ in range(len(prices))]
        for j in range(k):
            dp[0][2*j+1] = -prices[0]
        
        for i in range(1,len(prices)):
            for j in range(k):
                dp[i][2*j+1] = max(dp[i-1][2*j+1],dp[i-1][2*j]-prices[i])
                dp[i][2*j+2] = max(dp[i-1][2*j+2],dp[i-1][2*j+1]+prices[i])

        return dp[-1][-1] if dp[-1][-1] > 0 else 0
```

## [309.最佳买卖股票时机含冷冻期](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

### 思路
四种状态，不持有股票，当天买入或持有，当天卖出，冷冻期；
1. 不持有股票最大利润显然是前一天不持有或冷冻期或卖出后的最大利润；
2. 买入或持有的最大利润为前一天买入或持有，或当天买入的最大利润(前一天不持有或冷冻期)；
3. 当天卖出的最大利润是前一天就卖出或前天持有当天卖出的最大利润；
4. 冷冻期的最大利润为前一天卖出的最大利润（冷冻期的定义）。

最终最大利润一定是卖出股票状态的最大利润。

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp = [[0]*4 for _ in range(len(prices))]
        dp[0][0] = 0 # 不持有
        dp[0][1] = -prices[0] # 买入或持有
        dp[0][2] = 0 # 卖出
        dp[0][3] = 0 # 冷冻期
        for i in range(1,len(prices)):
            dp[i][0] = max(dp[i-1][0],dp[i-1][2],dp[i-1][3])
            dp[i][1] = max(dp[i-1][1],dp[i-1][0]-prices[i],dp[i-1][3]-prices[i])
            dp[i][2] = max(dp[i-1][2],dp[i-1][1]+prices[i])
            dp[i][3] = dp[i-1][2]
        return dp[-1][2] if dp[-1][2]>0 else 0
```

## [714.买卖股票的最佳时机含手续费](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

### 思路
和买卖股票的最佳时机Ⅱ类似，在卖出时加上手续费就可以了。
```python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        dp = [[0]*2 for _ in range(len(prices))]
        dp[0][0] = 0
        dp[0][1] = -prices[0]
        for i in range(1,len(prices)):
            dp[i][0] = max(dp[i-1][0],dp[i-1][1]+prices[i]-fee)
            dp[i][1] = max(dp[i-1][1],dp[i-1][0]-prices[i])
        return dp[-1][0] if dp[-1][0]>0 else 0
```
