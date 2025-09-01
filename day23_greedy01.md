# 贪心 part1
## [455.分发饼干](https://leetcode.cn/problems/assign-cookies/)

### 思路
饼干和胃口是一一对应的。  
可以选择优先喂饱胃口小的或者胃口大：
1. 先喂饱胃口小的，从最小饼干开始遍历，直到饼干大小超过最小胃口，然后二者同时加一，否则只是饼干+1；  
2. 先喂饱胃口大的，那么从最大胃口开始遍历，直到最大饼干大于胃口，然后同时减去一，继续遍历。


```python
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        g.sort()
        s.sort()
        index = 0
        for i in range(len(s)):
            if index < len(g) and s[i] >= g[index]:
                index += 1
        return index
```
从胃口大的开始遍历：
```python
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        g.sort()
        s.sort()
        index = len(s)-1
        result = 0
        for i in range(len(g)-1, -1, -1):
            if index >= 0 and s[index] >= g[i]:
                result += 1
                index -= 1
        return result
```

## [376. 摆动序列](https://leetcode.cn/problems/wiggle-subsequence/)

### 思路

尽可能保留峰值，而删除峰值之间的元素，则可以获得最长的摆动序列, 那么只需要统计摆动出现的次数+1就可以了。

```python
class Solution:
    def wiggleMaxLength(self, nums: List[int]) -> int:
        if len(nums) <= 1:
            return len(nums)
        curDiff = 0
        preDiff = 0
        result = 1
        for i in range(len(nums)-1):
            curDiff = nums[i+1] - nums[i]
            if (preDiff <= 0 and curDiff > 0) or (preDiff  >= 0 and curDiff < 0):
                result += 1
                preDiff = curDiff
        return result
```

## [53. 最大子序和](https://leetcode.cn/problems/maximum-subarray/description/)

### 思路

当当前连续和大于最大连续和，更新最大连续和。  
连续和小于等于0， 则抛弃当前子序列，从新index开始。

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        result = float("-inf")
        count = 0
        for i in range(len(nums)):
            count += nums[i]
            if count > result:
                result = count
            if count <= 0:
                count = 0
        return result

```