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

## [279. 完全平方数](https://leetcode.cn/problems/perfect-squares/description/)

### 思路
背包容量为n，可选数字为int(sqrt(n))，  
初始化dp[0] = 0  
由于是求最小数量，递推公式:  
$dp[j] = min(dp[j], dp[j-i*i]+1) $

```python
class Solution:
    def numSquares(self, n: int) -> int:
        dp = [float('inf')] * (n+1)
        dp[0] = 0
        for j in range(1,n+1):
            i=1
            while i*i <= j:
                dp[j] = min(dp[j], dp[j-i*i]+1)
                i += 1
        return dp[n]
```

## [139.单词拆分](https://leetcode.cn/problems/word-break/)

### 思路

错误的解法，每次只找到第一个匹配的单词，而放弃后面的，可能导致后续无法匹配成功。

```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        i = len(s)
        while i > 0:
            for word in wordDict: 
                if s[i-len(word):i] == word:
                    i -= len(word)
                    break
        return i == 0
```

使用递推，递推的参数显然是要求的s是否存在可拆分的方法，存在为True;  
递推公式建立在dp[i] == True 以及s[i:j] in wordDict中，则dp[j] = True;  
初始化：dp[0] = True, 空字符串不需要放入单词自然是True。


```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        dp = [False] * (len(s)+1)
        dp[0] = True
        for i in range(1,len(s)+1):
            for j in range(i):
                if dp[j] and s[j:i] in wordDict:
                    dp[i] = True
        return dp[-1]
```

## [多重背包 56. 携带矿石资源](https://programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%A4%9A%E9%87%8D%E8%83%8C%E5%8C%85.html#%E5%A4%9A%E9%87%8D%E8%83%8C%E5%8C%85)


### 题目描述
你是一名宇航员，即将前往一个遥远的行星。在这个行星上，有许多不同类型的矿石资源，每种矿石都有不同的重要性和价值。你需要选择哪些矿石带回地球，但你的宇航舱有一定的容量限制。 

给定一个宇航舱，最大容量为 C。现在有 N 种不同类型的矿石，每种矿石有一个重量 w[i]，一个价值 v[i]，以及最多 k[i] 个可用。不同类型的矿石在地球上的市场价值不同。你需要计算如何在不超过宇航舱容量的情况下，最大化你所能获取的总价值。

### 输入描述
输入共包括四行，第一行包含两个整数 C 和 N，分别表示宇航舱的容量和矿石的种类数量。 

接下来的三行，每行包含 N 个正整数。具体如下： 

第二行包含 N 个整数，表示 N 种矿石的重量。 

第三行包含 N 个整数，表示 N 种矿石的价格。 

第四行包含 N 个整数，表示 N 种矿石的可用数量上限。

### 输出描述
输出一个整数，代表获取的最大价值。
输入示例
10 3
1 3 4
15 20 30
2 3 2
### 输出示例
90
### 数据范围：
1 <= C <= 2000;
1 <= N <= 100;
1 <= w[i], v[i], k[i] <= 1000;

### 思路
将完全背包的k[i]个物体铺开，转化为01背包就可以了。
因为是组合，故而优先遍历物品，再从大到小遍历背包容量，避免重复拿取，最后遍历同意物品的数量。
python代码超时，不知道为什么==
```python
c, n = input().split(" ")
c, n = int(c),int(n)
w = [int(x) for x in input().split(" ")]
v = [int(x) for x in input().split(" ") if x]
k = [int(x) for x in input().split(" ")]

dp = [0] * (c+1)
for i in range(n):
    for j in range(c, w[i]-1,-1):
        for l in range(1,k[i]+1):
            if j >= w[i]*l:
                dp[j] = max(dp[j],dp[j-l*w[i]]+l*v[i])
            else:
                break
print(dp[-1])
```