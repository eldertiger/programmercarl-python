# 动态规划 part11
## [1143.最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/description/)

### 思路
使用`dp[i][j]`数组表示两个字符串字符`0~i-1, 0~j-1`位置得最长公共子序列的长度；  
递推关系：
1. 当i-1和j-1字符串相等时：`dp[i][j] = dp[i-1][j-1] + 1` 即比较前一组字符的情况。
2. 当二者不等时：`dp[i][j] = max(dp[i-1][j],dp[i][j-1])` 即看上方和左侧的的序列情况

初始化自然是0，注意下标从1开始，方便计算。

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        dp = [[0] * (len(text2)+1) for _ in range(len(text1)+1)]
        res = 0
        for i in range(1,len(text1)+1):
            for j in range(1,len(text2)+1):
                if text2[j-1] == text1[i-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i-1][j],dp[i][j-1])
                res = max(res, dp[i][j])
        return res
```

## [1035.不相交的线](https://leetcode.cn/problems/uncrossed-lines/description/)
### 思路
和最长公共子序列一样，连线不交叉就是求最长公共子序列。
```python
class Solution:
    def maxUncrossedLines(self, nums1: List[int], nums2: List[int]) -> int:
        dp  = [[0]*(len(nums2)+1) for _ in range(len(nums1)+1)]
        res = 0
        for i in range(1,len(nums1)+1):
            for j in range(1,len(nums2)+1):
                if nums1[i-1] == nums2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i-1][j],dp[i][j-1])
                res = max(res, dp[i][j])
        return res
```
## [53. 最大子序和](https://leetcode.cn/problems/maximum-subarray/description/)

### 贪心

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

### 动态规划
`dp[i]`表示以i为结尾的`0-i`的子数组的最大子数组和的大小；  
递推关系dp[i]等于dp[i-1]加上当前数字或者就从当前数字开始的最大值；  
初始化自然是第一个数字。

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        dp = [0]*len(nums)
        dp[0] = nums[0]
        res = dp[0]
        for i in range(1, len(nums)):
            dp[i] = max(dp[i-1]+nums[i], nums[i])
            res = max(res,dp[i])
        return res 
```

## [392.判断子序列](https://leetcode.cn/problems/is-subsequence/description/)
### 动态规划
在最长子序列和的基础上加上判断是否大于子序列的长度，是的话则跳出，注意此处不等的话，dp值应该是比较长字符串的上一个值和当前s的值`dp[i][j-1]`, 不需要和`dp[i-1][j]`比较，因为隐含条件前面的字符都匹配上了；
预先判断s是否空字符串，返回True。
```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        if len(s) == 0: return True
        dp =[[0]*(len(t)+1) for _ in range(len(s)+1)]
        for i in range(1,len(s)+1):
            for j in range(1, len(t)+1):
                if s[i-1] == t[j-1]:
                    dp[i][j] = dp[i-1][j-1]+1
                else:
                    dp[i][j] = dp[i][j-1]
                if dp[i][j] ==len(s):
                    return True
        return False
```

### 双指针

```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        if len(s) == 0: return True
        i, j = 0, 0
        while j < len(t) and i < len(s):
            if s[i] == t[j]:
                i += 1
                j += 1
            else:
                j += 1
        return True if i == len(s) else False
```