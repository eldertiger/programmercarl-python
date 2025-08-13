# 字符串 part1
## 一、  [344.反转字符串](https://leetcode.cn/problems/reverse-string/description/) 
### 方法：
交换首尾两个字符，直到交换到最中间的字符，采用双指针，每次向中间移动一位，直到左指针超过右指针，则交换完毕。
1. 时间复杂度$O(n)$
2. 空间复杂度$O(1)$
```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left, right = 0, len(s) - 1
        while left < right:
            s[left],s[right] = s[right], s[left]
            left += 1
            right -= 1
        return s
```

## 二、 [541. 反转字符串 II](https://leetcode.cn/problems/reverse-string-ii/description/)
### 方法：

基于反转字符的函数，每个2k区间，对前k个值进行翻转即可，避免逐个递增检查，python不需要考虑下标越界问题。

```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        def reverse(s):
            left, right = 0, len(s)-1
            while left < right:
                s[left], s[right] = s[right], s[left]
                left += 1
                right -= 1
            return s
        s = list(s)        
        for i in range(0, len(s), 2*k):
            s[i:i+k] = reverse(s[i:i+k])
        return ''.join(s)
```
## 三、 [替换数字](https://kamacoder.com/problempage.php?pid=1064)

将s转化为列表，替换其中的数字字符为number即可。
```python
def main():
    s = list(input())
    for i in range(len(s)):
        if '0'<=s[i]<='9':
            s[i] = 'number'
    print( ''.join(s))
main()
```