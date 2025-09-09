# 动态规划03

## [46. 携带研究材料](https://kamacoder.com/problempage.php?pid=1046)

### 题目描述
小明是一位科学家，他需要参加一场重要的国际科学大会，以展示自己的最新研究成果。他需要带一些研究材料，但是他的行李箱空间有限。这些研究材料包括实验设备、文献资料和实验样本等等，它们各自占据不同的空间，并且具有不同的价值。 

小明的行李空间为 N，问小明应该如何抉择，才能携带最大价值的研究材料，每种研究材料只能选择一次，并且只有选与不选两种选择，不能进行切割。

### 输入描述
第一行包含两个正整数，第一个整数 M 代表研究材料的种类，第二个正整数 N，代表小明的行李空间。

第二行包含 M 个正整数，代表每种研究材料的所占空间。 

第三行包含 M 个正整数，代表每种研究材料的价值。

### 输出描述
输出一个整数，代表小明能够携带的研究材料的最大价值。
### 输入示例
6 1
2 2 3 1 5 2
2 3 1 5 4 3
### 输出示例
5
### 提示信息
小明能够携带 6 种研究材料，但是行李空间只有 1，而占用空间为 1 的研究材料价值为 5，所以最终答案输出 5。 

#### 数据范围：
1 <= N <= 5000
1 <= M <= 5000
研究材料占用空间和价值都小于等于 1000



### 思路 二维dp
使用暴力法，每种材料有放或者不放两种情况，复杂度为$O(2^n)$;  
使用动态规划，对于要使用第i种材料和j的背包容量的最大价值$dp[i][j]$，若不放当前材料，则最大价值为$dp[i-1][j]$，放当前材料，则等于当前材料的价值加上$dp[i-1][j-space[i]]$的最大价值，即剩余空间为$j-space[i]$时不放当前材料的最大价值。  
那么递推公式为(j >= space[i])：

$dp[i][j] = max(dp[i-1][j], dp[i-1][j-space[i]]+value[i]) $

初始化dp, 所有值初始化为0；
对于第1种材料，那么价值就是第一种材料的价值(在背包容量大于等于第1种的空间时)

```python
types, bagspace = map(int, input().split())
space = list(map(int, input().split()))
value = list(map(int, input().split()))
dp = [[0]*(bagspace+1) for _ in range(types)]
for j in range(space[0],bagspace+1):
    dp[0][j] = value[0]

for i in range(1,types):
    for j in range(1, bagspace+1):
        if j>=space[i]:
            dp[i][j] = max(dp[i-1][j], dp[i-1][j-space[i]]+value[i])
        else:
            dp[i][j] = dp[i-1][j]
print(dp[-1][-1])
```

### 思路 一维dp

考虑使用一维递推数组dp,对背包容量对应的最大价值进行递推。  
递推公式为:
```python
for i in range(types):
    dp[j] = max(dp[j-1],dp[j-space[i]]+value[i]) 
```
dp初始化自然是$dp[0]=0$
此处遍历是对所有的种类以及所有大于等于当前种类空间的背包容量进行遍历，背包容量从大到小遍历，这样就可以避免多次存放同一个材料，因为使用的是存放上个材料的最大值，而非当前材料。

```python
types, bagspace = map(int, input().split())
space = list(map(int, input().split()))
value = list(map(int, input().split()))
dp = [0]*(bagspace+1)
for i in range(types):
    for j in range(bagspace,space[i]-1,-1):
        dp[j] = max(dp[j], dp[j-space[i]]+value[i])
print(dp[-1])
```

## [416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/description/)

### 思路
相当于将所有数字放入数字总和一半的背包中。
```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        sum_nums = sum(nums)
        if sum_nums%2 != 0: 
            return False
        target = sum_nums//2
        dp = [0]*(target+1)
        for num in nums:
            for j in range(target,num-1, -1):
                dp[j] = max(dp[j],dp[j-num]+num)
        return dp[-1] == target

```

错误思路： 通过排序计算从小到大的和判断，很明显，对于1234这样的是不可行的。