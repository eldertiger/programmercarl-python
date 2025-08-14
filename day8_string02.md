# 字符串part2

## 一、[151. 反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/description/)

### 方法1：双指针1
首先去除头尾空格，  
因为需要反转字符串，所以从尾到头寻找单词。  
从尾部到头部，找到第一个单词所有字符的位置，即不等于空格的位置，输出第一个单词，然后遍历空格直到下一个字符的位置，再次寻找单词，直到字符串头部。  
1. 时间复杂度$O(n)$
2. 空间复杂度$O(n)$
```python
class Solution:
    def reverseWords(self, s: str) -> str:
        s = s.strip()
        i = j = len(s)-1
        res =[]
        while i>=0:
            while i>=0 and s[i]!=' ': i-=1
            res.append(s[i+1:j+1])
            while i>=0 and s[i]==' ': i-=1
            j=i
        return ' '.join(res)
```

### 方法2：双指针2
从头到尾，然后逆序res即可。
```python
class Solution:
    def reverseWords(self, s: str) -> str:
        i = 0
        res = []
        n = len(s)
        while i < n:      
            if s[i]==' ':
                i += 1
            else:
                j = i 
                while j<=n-1 and s[j] != ' ': j +=1
                res.append(s[i:j])
                i = j
        return ' '.join(res[::-1])
```

## 二、 [55. 右旋字符串](https://kamacoder.com/problempage.php?pid=1065)
### 方法：
对于python还是list化，然后直接根据索引拼接。
```python
index = int(input())
s = input()
print(s[-index:]+s[:-index])
```

## 三、 [28. 实现 strStr()](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)
### 方法1：暴力匹配法：
首先判断needle是否长于haystack；  
从0开始，若遇到字符与needle起始字符相同，进入匹配，遍历m个字符，若全部相同，则输出起始字符的位置；否则跳出到下一个i。
1. 时间复杂度$O(nm)$
2. 空间复杂度$O(1)$

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        n, m = len(haystack),len(needle)
        if m>n: return -1
        for i in range(n):
            if haystack[i] == needle[0]:
                j = 0
                while j<m:
                    if i+j<n and haystack[i+j] == needle[j]:
                        j += 1
                    else:
                        break
                if j == m: 
                    return i
        return -1
```

### 方法2： KMP
首先建立前缀表，此处采用不位移的前缀表：  
然后利用前缀表，对字符串进行匹配，遇到不匹配的，回退到上一个字符前缀表对应的needle字符串位置继续匹配。

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        def getnext(s):
            j = 0
            n = len(s)
            prefix = [j] * n
            for i in range(1, n):
                while j>0 and s[i] != s[j]:
                    j = prefix[j-1]
                if s[j] == s[i]:
                    j += 1
                prefix[i] = j
            return prefix

        if len(needle) == 0: return 0
        prefix = getnext(needle)
        j = 0
        for i in range(len(haystack)):
            while j > 0 and haystack[i] != needle[j]:
                j = prefix[j - 1]
            if haystack[i] == needle[j]:
                j += 1
            if j == len(needle):
                return i - len(needle) + 1
        return -1
```
右移前缀表：
```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        def getnext(s):
            j = -1
            n = len(s)
            prefix = [j] * n
            for i in range(1, n):
                while j>=0 and s[i] != s[j+1]:
                    j = prefix[j]
                if s[i] == s[j+1]:
                    j += 1
                prefix[i] = j
            return prefix

        if len(needle) == 0: return 0
        prefix = getnext(needle)
        j = -1
        for i in range(len(haystack)):
            while j >= 0 and haystack[i] != needle[j+1]:
                j = prefix[j]
            if haystack[i] == needle[j+1]:
                j += 1
            if j == len(needle)-1:
                return i - len(needle) + 1
        return -1
```


## 四、 [459.重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/description/)

### 方法1：暴力法
遍历所有可能的子串长度(1~n//2), 若i满足子串重复，则j对应的j-i的值对于所有的j的位置都成立，则重复子字符串成立；否则不成立。
1. 时间复杂度$O(nm)$
2. 空间复杂度$O(1)$
```python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        n = len(s)
        for i in range(1, n//2+1):
            if n%i == 0:
                if all(s[j]==s[j-i] for j in range(i,n)):
                    return True
        return False
```

### 方法2： KMP
若由重复子字符串构成，则字符串可以整除(字符串长度-最长公共前后缀的长度)
```python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:  
        if len(s) == 0:
            return False
        nxt = [0] * len(s)
        self.getNext(nxt, s)
        if nxt[-1] != -1 and len(s) % (len(s) - (nxt[-1] + 1)) == 0:
            return True
        return False
    
    def getNext(self, nxt, s):
        nxt[0] = -1
        j = -1
        for i in range(1, len(s)):
            while j >= 0 and s[i] != s[j+1]:
                j = nxt[j]
            if s[i] == s[j+1]:
                j += 1
            nxt[i] = j
        return nxt
```