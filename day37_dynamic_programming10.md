# 动态规划 part10

## [300.最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/description/)

### 动态规划
以nums【i】为结尾的最长递增子序列的长度可以由 nums【0】为结尾的最长递增子序列长度、nums[1为结尾的最长长度、……nums【i-1】为结尾的最长长度 比较得到；  
dp的含义是从0到当前i index的数组以$nums[i]$结尾的最长递增子序列。注意nums[i]结尾，不然就没有比较意义了；  
递推公式为dp[i]等于i之前所有nums[j]小于nums[i]的dp[j]+1的最大值；  
初始化自然都是1；  
另外需要一个res保存最长子序列的长度(再次注意dp[i]是以nums[i]结尾的)

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return 1
        dp = [1]*len(nums)
        res = 1
        for i in range(1,len(nums)):
            for j in range(0,i):
                if nums[j]<nums[i]:
                    dp[i] = max(dp[i],dp[j]+1)
            res = max(res,dp[i])
        return res
```

### 贪心
维护一个有序的tails数组，其中tails[k]表示k+1的tails数组的尾部元素值，当遇到num小于等于tails[k]，则将num通过二分查找插到合适的位置，替换原有值，否则更新tails[k+1]为num,不断更新tails，最终输出tails的长度。

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        tails, res = [0]*len(nums), 0
        for num in nums:
            i, j = 0, res
            while i < j :
                m = (i+j)//2
                if tails[m] < num: i = m +1
                else: j=m
            tails[i] = num
            if j == res: res +=1
        return res
```

## [674. 最长连续递增序列](https://leetcode.cn/problems/longest-continuous-increasing-subsequence/)

### 动态规划
dp含义为以当前值为结尾的最长连续递增序列；  
由于需要连续，所以每次只需要对比当前值和前面的值，若大于前面的值，则最大长度加一， 否则最大长度归一。

```python
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        dp = [1]*len(nums)
        for i in range(1,len(nums)):
            if nums[i]>nums[i-1]:
                dp[i] = dp[i-1]+1
            else:
                dp[i] = 1
        return max(dp)
```

## [718. 最长重复子数组](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/)

### 动态规划思路
`dp[i][j]`表示以`i-1, j-1`结尾的最长重复连续子序列的长度；  
递推关系为若当前i-1,j-1对应下标的两个num相等，则：
`dp[i][j] = dp[i-1][j-1]+1`

初始化自然都初始化为0。


```python
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        dp = [[0]*(len(nums2)+1) for _ in range(len(nums1)+1)]
        res = 0
        for i in range(1, len(nums1)+1):
            for j in range(1, len(nums2)+1):
                if nums1[i-1] == nums2[j-1]:
                    dp[i][j] = dp[i-1][j-1]+1
                res = max(res,dp[i][j])
        return res
```