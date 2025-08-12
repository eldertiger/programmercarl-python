# 哈希表 part2
## 一、 [454. 四数相加 II](https://leetcode.cn/problems/4sum-ii/description/)
### 方法：
将四数相加变为两个两数相加，复杂度从$O(n^4)$变为$O(n^2)$

```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        dic = defaultdict(int)
        for i in range(len(nums1)):
            for j in range(len(nums2)):
                dic[nums1[i]+nums2[j]] +=1
        count = 0
        for k in range(len(nums3)):
            for l in range(len(nums4)):
                if 0-nums3[k]-nums4[l] in dic:
                    count += dic[0-nums3[k]-nums4[l]] 
        return count
```

## 二、 [383. 赎金信](https://leetcode.cn/problems/ransom-note/description/)

### 方法：
先遍历一遍ransomNote，存为字典，值-出现次数，再遍历一遍magazine，若所有字典中的值的出现次数可以减为0或者负数，则说明ransomNote 能由 magazine 里面的字符构成。
1. 时间复杂度$(O(n))$
2. 空间复杂度$(O(1))$

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        dic = defaultdict(int)
        for c in ransomNote:
            dic[c] += 1
        for c in magazine:
            if c in dic:
                dic[c] -=1
        for c in dic:
            if dic[c]>0:
                return False
        return True
```