# 动态规划 part9

## [115. 不同的子序列](https://leetcode.cn/problems/distinct-subsequences/)
### 思路
递归`dp[i][j]`含义为以`i-1`结尾的`t`在`0~j-1`结尾的`s`中的个数；  
递推关系，对于`s[j-1]`等于`t[i-1]`的情况，`dp[i][j]`的个数为使用当前`t[i]`和不使用当前`t[i]`的去匹配`0~j-1`的个数之和；若不相等，则直接等于使用当前`t[i]`匹配`0~j-1`的个数之和;  
初始化，显然空字符串总是子序列，个数为1；故而`dp[0][:] = 1`

```python
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        dp = [[0] * (len(s)+1) for _ in range(len(t)+1)]
        for j in range(len(s)+1):
            dp[0][j] = 1
        for i in range(1,len(t)+1):
            for j in range(1, len(s)+1):
                if s[j-1] == t[i-1]:
                    dp[i][j] = dp[i-1][j-1]+dp[i][j-1]
                else:
                    dp[i][j] = dp[i][j-1]
        return dp[-1][-1]
```

## [583. 两个字符串的删除操作](https://leetcode.cn/problems/delete-operation-for-two-strings/description/)
### 最长公共子序列

本题就是求最长公共子序列的长度，然后剩下的就是需要删除的长度

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        dp = [[0]*(len(word2)+1) for _ in range(len(word1)+1)]
        for i in range(1,len(word1)+1):
            for j in range(1,len(word2)+1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]+1
                else:
                    dp[i][j] = max(dp[i-1][j],dp[i][j-1])
        return len(word1)+len(word2) - 2*dp[-1][-1]
```
### 删除
`dp[i][j]`代表`0~i-1`，`0~j-1`的以i-1,j-1为结尾的两个字符串变为相同需要的删除操作数；  
1. 当两个字符相等时，删除操作数和上一组字符相同； 
2. 不等时，需要删除一个元素重新匹配，相当于从(i-1,j) (i,j-1)分别多操作一次删除；

初始化时，显然要变成空字符串就分别需要i,j步。


```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        dp = [[0]*(len(word2)+1) for _ in range(len(word1)+1)]
        for j in range(len(word2)+1):
            dp[0][j] = j
        for i in range(len(word1)+1):
            dp[i][0] = i
        for i in range(1,len(word1)+1):
            for j in range(1,len(word2)+1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i-1][j]+1,dp[i][j-1]+1)
        return  dp[-1][-1]
```


## [72. 编辑距离](https://leetcode.cn/problems/edit-distance/description/)

### 思路

本题和上题类似，多了替换和插入操作，相等时，递推关系一样，不等时，可以使用替换，这样就可以从`dp[i-1][j-1]`变为`dp[i][j]`只需要一步。


```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        dp = [[0]*(len(word2)+1) for _ in range(len(word1)+1)]
        for i in range(len(word1)+1):
            dp[i][0] = i
        for j in range(len(word2)+1):
            dp[0][j] = j
        for i in range(1, len(word1)+1):
            for j in range(1, len(word2)+1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i-1][j]+1,dp[i][j-1]+1,dp[i-1][j-1]+1)
        return dp[-1][-1]
```