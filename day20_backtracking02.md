# 回溯 part2

## [39. 组合总和](https://leetcode.cn/problems/combination-sum/description/)

### 递归法

```python
class Solution:

    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        path = []
        res = []
        def backtracking(candidates, target, start_index):
            nonlocal res, path
            if target < 0: return
            if target == 0:
                res.append(path[:])
                return
            for i in range(start_index, len(candidates)):
                path.append(candidates[i])
                target -= candidates[i]
                backtracking(candidates, target,i)
                target += candidates[i]
                path.pop()
        backtracking(candidates, target, 0)
        return res
```

### 剪枝

排序后就可以剪枝了。  

```python
class Solution:

    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        path = []
        res = []
        def backtracking(candidates, target, start_index):
            nonlocal res, path
            if target < 0: return
            if target == 0:
                res.append(path[:])
                return
            for i in range(start_index, len(candidates)):
                if candidates[i] > target:
                    break
                path.append(candidates[i])
                target -= candidates[i]
                backtracking(candidates, target,i)
                target += candidates[i]
                path.pop()
        candidates.sort()
        backtracking(candidates, target, 0)
        return res
```

## [40.组合总和II](https://leetcode.cn/problems/combination-sum-ii/)

### 递归法

本题的关键在于如何判断同一层遍历中，相同的元素已经使用过。  
通过添加额外的使用过的数组used:  

1. 同一层中，如果当前元素和前面的元素相同，而`used==0`, 则说明前面的相同元素已经使用过了；  
2. 在同一数组中，若当前元素和前面的元素相同，而`used==1`，说明前面的相同元素使用过了，但是不影响继续使用当前元素。

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []
        path = []
        used = [0] * len(candidates)
        def backtracking(candidates, target, start_index):
            nonlocal res, path, used
            if target < 0: return
            if target == 0:
                res.append(path[:])
                return
            for i in range(start_index, len(candidates)):
                if candidates[i] > target:
                    break
                if i>0 and candidates[i] == candidates[i-1] and used[i-1] == 0:
                    continue
                path.append(candidates[i])
                target -= candidates[i]
                used[i] = 1
                backtracking(candidates, target, i+1)
                used[i] = 0
                target += candidates[i]
                path.pop()

        candidates.sort()
        backtracking(candidates, target, 0)
        return res
```

### 使用start_index去重

同一层中，当i大于start_index且当前元素等于前置元素，说明前置元素已经使用过了，需要跳过当前元素。  

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []
        path = []
        def backtracking(candidates, target, start_index):
            nonlocal res, path
            if target < 0: return
            if target == 0:
                res.append(path[:])
                return
            for i in range(start_index, len(candidates)):
                if candidates[i] > target:
                    break
                if i>start_index and candidates[i] == candidates[i-1]:
                    continue
                path.append(candidates[i])
                target -= candidates[i]
                backtracking(candidates, target, i+1)
                target += candidates[i]
                path.pop()

        candidates.sort()
        backtracking(candidates, target, 0)
        return res
```

## [131.分割回文串](https://leetcode.cn/problems/palindrome-partitioning/description/)

### 递归法

需要额外判断是否是回文串的函数，回溯主要体现在分割字符串的index上。

```python
class Solution:
    def isPalindrome(self, s, start, end):
        i = start
        j = end
        while i<j:
            if s[i] != s[j]:
                return False
            i += 1
            j -= 1
        return True
    def backtracking(self, s, start_index, path, res):
        if start_index == len(s):
            res.append(path[:])
            return
        for i in range(start_index, len(s)):
            if self.isPalindrome(s, start_index,i):
                path.append(s[start_index:i+1])
                self.backtracking(s, i+1, path, res)
                path.pop()                    
    
    def partition(self, s: str) -> List[List[str]]:
        path = []
        res = []
        self.backtracking(s, 0, path, res)
        return res
```

### 将backtracing写到里面

```python
class Solution:
    def isPalindrome(self, s, start, end):
        i = start
        j = end
        while i<j:
            if s[i] != s[j]:
                return False
            i += 1
            j -= 1
        return True
    def partition(self, s: str) -> List[List[str]]:
        path = []
        res = []
        def backtracking(s, start_index):
            nonlocal path, res
            if start_index == len(s):
                res.append(path[:])
                return
            for i in range(start_index, len(s)):
                if self.isPalindrome(s, start_index,i):
                    path.append(s[start_index:i+1])
                else:
                    continue
                backtracking(s, i+1)
                path.pop()
        backtracking(s,0)
        return res
```
