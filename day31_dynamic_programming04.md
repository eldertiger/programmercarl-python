# 动态规划part4

## [1049.最后一块石头的重量II](https://leetcode.cn/problems/last-stone-weight-ii/)

### 思路

本题相当于求最接近石头总重量一半的子石头组的重量。
和昨天最后的分割等和子集类似的思路。

```python
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        _sum = sum(stones)
        dp = [0]*(_sum//2+1)
        for stone in stones:
            for j in range(_sum//2,stone-1,-1):
                dp[j] = max(dp[j],dp[j-stone]+stone)
        return _sum-2*dp[-1]
```

## [494.目标和](https://leetcode.cn/problems/target-sum/)

### 思路
将nums分为需要分配正号和负号的子集，左边和为left，右边和为right，假设$left-right = target$， 返回过来也一样，则又有$left+right = sum$，可以得到  
$left = (target+sum)/2$  
则该问题可以抽象为向一个容量为(target+sum)/2的背包放nums中的元素，放满背包有多少方法。

对于容量为j的背包，放满的方法等于在背包中分别放体积小于容量的元素后，剩余容量可以放满的方法之和。即(i>num)：  
```python
for num in nums:
    dp[i] += dp[i-num] 
```
初始化对于dp[0]，放满0只有一种方法，就是不放，故而:
dp[0] = 1  
时间复杂度为$O(n*m)$, 空间复杂度$O(m)$ m为背包容量
```python
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        _sum = sum(nums)
        if (_sum+target)%2 == 1 or _sum < abs(target): return 0
        res = 0
        dp = [0]*((_sum+target)//2+1)
        dp[0] = 1
        for num in nums:
            for j in range((_sum+target)//2,num-1,-1):
                dp[j] += dp[j-num]
        return dp[-1]
```

## [474.一和零](https://leetcode.cn/problems/ones-and-zeroes/)

### 思路
二维的01背包问题，对于每个需要装进(m,n)背包的元素，其占用的0，1空间是固定的，对于递推公式$dp[i][j]$能够装入的最大容量应该为（对所有元素遍历，x,y 分别为元素0，1数量）：

$dp[i][j] = max(dp[i-x][j-y]+1,dp[i][j])$

```python
class Solution:

    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        dp = [[0] * (m+1) for _ in range(n+1)]
        # dp[0][0] = 0
        for _str in strs:
            x, y = 0, 0
            for c in _str:
                if c == "0": 
                    x += 1
                else:
                    y += 1
            for j in range(n,y-1,-1):
                for k in range(m,x-1,-1):
                    dp[j][k] = max(dp[j-y][k-x]+1,dp[j][k])
        return dp[-1][-1]
```