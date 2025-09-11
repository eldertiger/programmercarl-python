# 动态规划 part5

如果求组合数就是外层for循环遍历物品，内层for遍历背包。

如果求排列数就是外层for遍历背包，内层for循环遍历物品。

## [518.零钱兑换II](https://leetcode.cn/problems/coin-change-ii/)

### 二维动态规划
$dp[i][j]$ 表示放入零钱i或者不放入零钱i的容量为j的背包的零钱组合数量。

递推公式：  
$dp[i][j] = dp[i-1][j] + dp[i][j-coins[i]]$  
若$j < coins[i]$ ，则：  
$dp[i][j] = dp[i-1][j]$  
即放不下coins[i]，那还等于之前的零钱的组合数。

初始化：
i = 0 时，若j能整除coins[0]，则dp[0][j]的组合数量为1，否则为0，即装不满。  
j = 0 时，可以初始化为1，也可以初始化为0，建议初始化为1，便于理解，即容量为0的背包只有一种组合，就是空组合。

```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        dp = [[0]*(amount+1) for _ in range(len(coins))]
        for j in range(amount+1):
            if j % coins[0] == 0:
                dp[0][j] = 1
        for i in range(len(coins)):
            dp[i][0] = 1
        for i in range(1,len(coins)):
            for j in range(amount+1):
                if j<coins[i]:
                    dp[i][j] = dp[i-1][j]
                else:
                    dp[i][j] = dp[i-1][j] + dp[i][j-coins[i]]
        return dp[-1][-1]
```

### 一维递归

基于二维递归，不难写出如下有一维递归的方法。

```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        dp = [0]*(amount+1) 
        dp[0] = 1
        for i in range(0,len(coins)):
            for j in range(coins[i],amount+1):
                dp[j] += dp[j-coins[i]]
        return dp[-1]
```

## [377. 组合总和 Ⅳ](https://leetcode.cn/problems/combination-sum-iv/)

实质上题意是排列，将背包遍历放在外面，就可以重新加入物品了。
```python
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        dp = [0]*(target+1)
        dp[0] = 1
        for j in range(0,target+1):
            for i in range(len(nums)):
                if j>=nums[i]:
                    dp[j] += dp[j-nums[i]]
        return dp[-1]
```

## [70. 爬楼梯](https://programmercarl.com/0070.%E7%88%AC%E6%A5%BC%E6%A2%AF%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85%E7%89%88%E6%9C%AC.html)

和上一题类似，是可重复选取之前元素的排列，故而背包大小遍历在外面，元素(一次怕多少楼梯)遍历在里面。
初始化dp[0]=1, 表示不用爬就可以到0级台阶，没有实质意义，只是为了递推。

```python
n, m = map(int,input().split(" "))
dp = [0]*(n+1)
dp[0] = 1
for j in range(n+1):
    for i in range(1,m+1):
        if j>=i:
            dp[j] += dp[j-i]
print(dp[-1])
```