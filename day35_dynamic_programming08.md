# 动态规划 part8
## [121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

### 思路：
维护一个当前日期前得最小股票价格，那么当前日期能获得得最大利润一定是之前得最大利润或当前价格减去最小价格。

空间复杂度O(1), 时间复杂度O(n)

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        min_price = prices[0]
        max_profit = 0
        for price in prices:
            min_price = min(min_price, price)
            max_profit = max(max_profit, price - min_price)
        if max_profit > 0:
            return max_profit
        else:
            return 0
```
### 动态规划
使用 $dp[i][0]$ 表示不持有股票的最大利润，$dp[i][1]$ 表示第i天持有该股票的最大利润。  
那么不持有股票的最大利润应该等于前一天不持有的最大利润和当天卖出的最大利润，即  
$dp[i][0] = max(dp[i-1][0],dp[i-1][1]+prices[i])$  
持有股票的最大利润显然等于前一天持有的最大利润和当天买入的最大利润(注意只能买卖一次，所以买入前的利润为0)，即  
$dp[i][1] = max(dp[i-1][1], 0-prices[i])$  
初始化，第一天持有的利润即买入的价格负值，第一天不持有的利润显然为0。

空间复杂度O(n), 时间复杂度O(n)

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp = [[0]*2 for _ in range(len(prices))]
        dp[0][1] = -prices[0]
        dp[0][0] = 0 
        for i in range(1,len(prices)):
            dp[i][0] = max(dp[i-1][0],dp[i-1][1]+prices[i])
            dp[i][1] = max(dp[i-1][1],-prices[i])
        return dp[-1][0] if dp[-1][0] > 0 else 0
```

## [122. 买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/description/)

### 贪心思路
由于可以多次买入卖出，那么只需要考虑前后天有利可图的日子，即最大利润为所有后一天大于前一天价格的价差的和。
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        max_profit=0
        for i in range(1, len(prices)):
            if prices[i]>prices[i-1]:
                max_profit += prices[i]-prices[i-1]
        return max_profit
```

### 动态规划

和上题思路一样，除了当天买入的利润应该是前一天不持有的最大利润减去当天的价格(之前不持有就是0).

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp = [[0]*2 for _ in range(len(prices))]
        dp[0][1] = -prices[0]
        dp[0][0] = 0 
        for i in range(1,len(prices)):
            dp[i][0] = max(dp[i-1][0],dp[i-1][1]+prices[i])
            dp[i][1] = max(dp[i-1][1],dp[i-1][0]-prices[i])
        return dp[-1][0] if dp[-1][0] > 0 else 0
```

## [123. 买卖股票的最佳时机 III](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/description/)

### 思路
使用多个不同的状态来描述两次买入卖出。共有如下五种状态：
1. 无操作
2. 第一次买入
3. 第一次不持有
4. 第二次买入
5. 第二次不持有

所有状态最大利润都是前一天的最大利润和当天进行买卖后的最大利润的最大值。

初始化：
1. 无操作利润都是0；
2. 第一次买入和第二次买入均为-prices[0],后者可以理解为当天买了两次，卖了一次。
3. 第一天不持有利润都是0。

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp = [[0]*5 for _ in range(len(prices))]
        dp[0][0] = 0 # 无操作
        dp[0][1] = -prices[0] # 第一次买入
        dp[0][2] = 0 # 第一次不持有
        dp[0][3] = -prices[0]  # 第二次买入
        dp[0][4] = 0 # 第二次不持有
        for i in range(1,len(prices)):
            dp[i][0] = dp[i-1][0]
            dp[i][1] = max(dp[i-1][1],dp[i-1][0]-prices[i])
            dp[i][2] = max(dp[i-1][2],dp[i-1][1]+prices[i])
            dp[i][3] = max(dp[i-1][3],dp[i-1][2]-prices[i])
            dp[i][4] = max(dp[i-1][4],dp[i-1][3]+prices[i])
        return dp[-1][4] if dp[-1][4] > 0 else 0
```