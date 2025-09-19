# 动态规划 part13
## [647. 回文子串](https://leetcode.cn/problems/palindromic-substrings/)

### 思路：
dp数组表示i~j位置的子字符串是否为回文子串；
递推关系如下：
1. 若i、j位置的字符相等，且二者之差小于等于1，如a,aa,则明显是回文字符串；
2. 若i、j位置字符相等，二者之差大于1，则该则字符串是否回文却决于i+1~j-1位置是否是回文字符串；
3. 若i、j位置字符不等，则不会是回文字符串；

初始化dp为0或者False.  
由于条件2取决于左下角的dp值，所以从左下角向右上角遍历。

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        dp = [[0]*len(s) for _ in range(len(s))]
        res = 0
        for i in range(len(s)-1,-1,-1):
            for j in range(i,len(s)):
                if s[i] == s[j]:
                    if j-i <= 1:
                        dp[i][j] = 1
                        res += 1
                    elif dp[i+1][j-1] == 1:
                        dp[i][j] = 1
                        res += 1
        return res

```

## [516.最长回文子序列](https://leetcode.cn/problems/longest-palindromic-subsequence/description/)

### 思路
dp在此处含义为i~j位置的最长回文字符列长度；  
递推关系：  
1. 若i、j位置字符相同，则dp长度为dp[i+1][j-1]+2，即向外扩了两个字符；  
2. 若i、j位置字符不同，则dp为减少其中一个字符后dp的最大值。  
和上题类似，需要从左下到右上遍历；  
初始化需要初始化对角线的单个元素长度为1；  
遍历从len(s)-2开始，跳过最左下角的元素，j从i+1开始；


```python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        dp = [[0]*len(s) for _ in range(len(s))]
        res = 1
        for i in range(len(s)):
            dp[i][i] = 1
        for i in range(len(s)-2, -1, -1):
            for j in range(i+1,len(s)):
                if s[i]==s[j]:
                    dp[i][j] = dp[i+1][j-1]+2
                else:
                    dp[i][j] = max(dp[i+1][j],dp[i][j-1])
                res = max(res,dp[i][j])
        return res
```