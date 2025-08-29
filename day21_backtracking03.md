# 回溯 part3

## [93.复原IP地址](https://leetcode.cn/problems/restore-ip-addresses/description/)

### 递归法

首先需要一个判断有效ip的函数；  
递归传入参数为字符串和分割开始的位置start_index;  
递归退出条件，意识当前组成的ip长度大于4，2是start_index到达字符串末尾，这时若当前ip长度为4，则是有效的，可加入结果，否则是无效的。  
递归处理就是遍历start_index到len(s)，对字符串进行分割，直到找到有效的ip地址。

```python
class Solution:
    def __init__(self):
        self.path = []
        self.res = []
    def isValidIP(self,str):
        if len(str) > 3: return False
        if len(str) > 1 and str[0] == "0": return False
        if int(str) > 255: return False
        return True
    
    def backtracking(self,s,start_index):
        if len(self.path) > 4: 
            return
        if start_index == len(s):
            if len(self.path) == 4:
                self.res.append('.'.join(self.path))
                return
            else:
                return
        for i in range(start_index, len(s)):
            if self.isValidIP(s[start_index:i+1]):
                self.path.append(s[start_index:i+1])
                self.backtracking(s, i+1)
                self.path.pop()
        
    def restoreIpAddresses(self, s: str) -> List[str]:
        self.backtracking(s, 0)
        return self.res
```

### 加入额外的剪枝

遇到非有效字符则直接break当前循环，以及最开始判断下字符串的长度。

```python
class Solution:
    def __init__(self):
        self.path = []
        self.res = []
    def isValidIP(self,str):
        if len(str) > 3: return False
        if len(str) > 1 and str[0] == "0": return False
        if int(str) > 255: return False
        return True
    
    def backtracking(self,s,start_index):
        if len(self.path) > 4: 
            return
        if start_index == len(s):
            if len(self.path) == 4:
                self.res.append('.'.join(self.path))
                return
            else:
                return
        for i in range(start_index, len(s)):
            if self.isValidIP(s[start_index:i+1]):
                self.path.append(s[start_index:i+1])
                self.backtracking(s, i+1)
                self.path.pop()
            else:
                break
        
    def restoreIpAddresses(self, s: str) -> List[str]:
        if len(s) < 4 or len(s) > 12: return []
        self.backtracking(s, 0)
        return self.res
```

## [78.子集](https://leetcode.cn/problems/subsets/description/)

### 递归法

将res获取path内容提前到每个path获取新内容之后，而非组合的到达目标长度，由于没有重复数字，不用担心path重复。

```python
class Solution:
    def __init__(self):
        self.res =[]
        self.path = []
    def backtracking(self, nums, start_index):
        if start_index == len(nums):
            return
        for i in range(start_index, len(nums)):
            self.path.append(nums[i])
            self.res.append(self.path[:])
            self.backtracking(nums,i+1)
            self.path.pop()
    def subsets(self, nums: List[int]) -> List[List[int]]:
        self.res.append(self.path[:])
        self.backtracking(nums,0)
        return self.res
```

也可以将res获取path提前到backtracking一开始，这样就不用额外将空的path放入res中了：

```python
class Solution:
    def __init__(self):
        self.res =[]
        self.path = []
    def backtracking(self, nums, start_index):
        self.res.append(self.path[:])
        if start_index == len(nums):
            return
        for i in range(start_index, len(nums)):
            self.path.append(nums[i])
            self.backtracking(nums,i+1)
            self.path.pop()
    def subsets(self, nums: List[int]) -> List[List[int]]:
        self.backtracking(nums,0)
        return self.res
```

## [90.子集II](https://leetcode.cn/problems/subsets-ii/)

### 递归法

排序后，和之前组合一样判断重复数值，即当i大于start_index时，而前后nums相同，说明同一层已经使用过该数字了，跳过即可。

```python
class Solution:
    def __init__(self):
        self.res = []
        self.path = []
    def backtracking(self, nums, start_index):
        self.res.append(self.path[:])
        if start_index == len(nums):
            return
        for i in range(start_index, len(nums)):
            if i > start_index and nums[i] == nums[i-1]:
                continue
            self.path.append(nums[i])
            self.backtracking(nums, i + 1)
            self.path.pop()
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        self.backtracking(nums,0)
        return self.res
```