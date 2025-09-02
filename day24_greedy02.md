# 贪心 part2

## [122.买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/description/)

### 思路

由于可以当天买卖，只需要收集相对于前天的正收益就可以得到最大利润。

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        profit = 0
        for i in range(1, len(prices)):
            if prices[i] > prices[i-1]: profit += prices[i] - prices[i-1]
        return profit
```

## [55. 跳跃游戏](https://leetcode.cn/problems/jump-game/description/)

### 思路

查看每个元素的最远能跳的index，不断更新该index，若该index在某一处等于元素index，说明其无法前进了。  
否则，说明可以到达最后的index。
```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        max_index = 0
        for i in range(len(nums)-1):
            max_index = max(i+nums[i], max_index)
            if max_index == i:
                return False
        return True
```


## [45. 跳跃游戏 II](https://leetcode.cn/problems/jump-game-ii/)

### 思路


对于元素的覆盖范围，维护一个到达当前覆盖范围内跳的最远的位置。如果遍历到了之前的覆盖范围的尽头，则更新覆盖范围为最远的位置，意味着从之前那个位置开始跳，同时跳的次数加一。直到最后遍历到倒数第2个值，在访问最后一个元素之前，我们的最远位置一定大于等于最后一个位置(题设满足)。

```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        if len(nums) == 0: return 0
        last_max_index = 0
        next_max_index = 0
        count = 0
        for i in range(len(nums)-1):
            next_max_index = max(i+nums[i], next_max_index)
            if i == last_max_index:
                count += 1
                last_max_index = next_max_index
        return count
```

## [1005.K次取反后最大化的数组和](https://leetcode.cn/problems/maximize-sum-of-array-after-k-negations/)

### 思路

按照绝对值大小降序排序，从左向右依次转换负值，若k值仍有剩余，则对绝对值最小的值进行剩余k次转换，求和即为结果。

```python
class Solution:
    def largestSumAfterKNegations(self, nums: List[int], k: int) -> int:
        nums.sort(key=lambda x: abs(x), reverse=True)
        for i in range(len(nums)):
            if nums[i] < 0 and k >0:
                nums[i] = -nums[i]
                k -= 1
        if k%2 == 1:
            nums[-1] = -nums[-1]
        return sum(nums)
```