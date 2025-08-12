# 哈希表 part1
## [242.有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)
### 方法一：
使用数组存储26个字母，对于每个s中字符，统计其出现频次，对于t中的字符，删除其频次，如数组中有字符频次不为零，则不是字母异位词，否则，则返回True。
1. 时间复杂度$O(n)$，遍历了两次字符串，以及一次常数数组；  
2. 空间复杂度$O(n)$，使用了常数大小的数组计数。
```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        record = [0]*26
        for c in s:
            record[ord(c)-ord("a")] += 1
        for c in t:
            record[ord(c)-ord("a")] -= 1
        for i in range(len(record)):
            if record[i] != 0:
                return False
        return True
```

### 方法二：
使用defaultdict

```python
from collections import defaultdict
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        s_dict = defaultdict(int)
        t_dict = defaultdict(int)
        for c in s:
            s_dict[c] += 1
        for c in t:
            t_dict[c] += 1
        return s_dict == t_dict    
```

## 二、 [349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/description/)

### 方法1： 暴力法
直接对于num2中的每一个元素，判断是否在num1中且不在stack中，压入栈，最后返回栈即可。
1. 时间复杂度$O(mn)$
```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        stack = []
        for num in nums2:
            if num not in stack and num in nums1:
                stack.append(num)
        return stack
```
### 方法2：使用字典
```python
from collections import defaultdict
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        dic = defaultdict(int)
        for num in nums1:
            dic[num] += 1
        
        # 使用集合存储结果
        res = set()
        for num in nums2:
            if num in dic:
                res.add(num)
                del dic[num]
        return list(res)
```

### 方法3： 使用集合

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return list(set(nums1) & set(nums2))
```


## 三、 [第202题. 快乐数](https://leetcode.cn/problems/happy-number/description/)
### 方法：
要判断不是快乐数，则要记下每次各位数平方和的值到集合，若重复出现，则不是快乐数，若变为1，则为快乐数；  
1. 时间复杂度$O(log n)$
2. 空间复杂度$O(log n)$

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        def square_n(n):
            res = 0
            while n:
                b = n%10
                n = n//10
                res += b**2
            return res
        dic = set()
        while n>1:
            if n not in dic:
                dic.add(n)
                n = square_n(n)
            else:
                return False
        return True
```
### 关于复杂度的证明: 
此处引用leetcode官方题解。

|Digits |Largest	|Next|
|---:|---:|---:|
|1	| 9	| 81 |
|2	| 99|	162|
|3	|999|	243|
|4	|9999|	324|
|13	|9999999999999|	1053|

对于 3 位数的数字，它不可能大于 243。这意味着它要么被困在 243 以下的循环内，要么跌到 1。4 位或 4 位以上的数字在每一步都会丢失一位，直到降到 3 位为止。所以我们知道，最坏的情况下，算法可能会在 243 以下的所有数字上循环，然后回到它已经到过的一个循环或者回到 1。

#### 1. 时间复杂度：O(243⋅3+logn+loglogn+logloglogn)... = O(logn)
查找给定数字的下一个值的成本为 O(logn)，因为我们正在处理数字中的每位数字，而数字中的位数由 logn 给定。  
要计算出总的时间复杂度，我们需要仔细考虑循环中有多少个数字，它们有多大。  
我们在上面确定，一旦一个数字低于 243，它就不可能回到 243 以上。因此，我们就可以用 243 以下最长循环的长度来代替 243，不过，因为常数无论如何都无关紧要，所以我们不会担心它。  
对于高于 243 的 n，我们需要考虑循环中每个数高于 243 的成本。通过数学运算，我们可以证明在最坏的情况下，这些成本将是 O(logn)+O(loglogn)+O(logloglogn)...。幸运的是，O(logn) 是占主导地位的部分，而其他部分相比之下都很小（总的来说，它们的总和小于logn），所以我们可以忽略它们。 

#### 2. 空间复杂度：O(logn)
与时间复杂度密切相关的是衡量我们放入哈希集合中的数字以及它们有多大的指标。对于足够大的 n，大部分空间将由 n 本身占用。我们可以很容易地优化到 O(243⋅3)=O(1)，方法是只保存集合中小于 243 的数字，因为对于较高的数字，无论如何都不可能返回到它们。



## 四、 [1. 两数之和](https://leetcode.cn/problems/two-sum/description/)

### 方法：
使用字典存储值和对应的index，对于每个num值，寻找target-num是否存在于字典中，若存在则返回二者的index，否则，则返回空；
1. 时间复杂度$O(n)$
2. 空间复杂度$O(n)$
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        dic = defaultdict(int)
        for index, num in enumerate(nums):
            if target-num  in dic:
                return [index, dic[target-num]]
            dic[num] = index
        return []
```