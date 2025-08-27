# 回溯法 part1

## [77. 组合](https://leetcode.cn/problems/combinations/description/)

### 方法：递归法

递归传入参数为n, k和起始的index，递归退出条件为path的长度等于k;  
注意res添加path需要添加[:], 否则只是添加了path的指针，而非值的复制。

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        res = []
        path = []
        def backtracking(n, k, start_index):
            nonlocal res,path
            if len(path) == k: 
                res.append(path[:])
                return
            for i in range(start_index, n):
                path.append(i)
                backtracking(n,k,i+1)
                path.pop()
            
        backtracking(n, k, 1)
        return res
```

#### 剪枝优化

第一层循环不需要循环到4，只需要循环到n-k+1，因为后面的值数量不够了，下一层同理。

```python
class Solution:

    def combine(self, n: int, k: int) -> List[List[int]]:
        res = []
        path = []
        def backtracking(n, k, start_index):
            nonlocal res,path
            if len(path) == k: 
                res.append(path[:])
                return
            for i in range(start_index,n-(k-len(path))+2):
                path.append(i)
                backtracking(n,k,i+1)
                path.pop()
        backtracking(n, k, 1)
        return res
```

## [216.组合总和III](https://leetcode.cn/problems/combination-sum-iii/description/)

### 方法：递归法

```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        res = []
        path = []
        if n > 45 or n < k*(k+1)/2 : return res
        def backtracking(k, n, start_index):
            nonlocal res, path
            if len(path) > k or n < 0: return
            if len(path) == k and n == 0:
                res.append(path[:])
                return
            for i in range(start_index, 9-(k-len(path))+2):
                path.append(i)
                n -= i
                backtracking(k, n, i+1)
                n += i
                path.pop()

        backtracking(k, n, 1)
        return res
```

## [17.电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

### 方法：递归法

递归的传入参数为组合的长度，字符串中数字的当前位置；  

因为数字对应的字母都不一样，所以递归所有的字符串数字对应的字母即可；

终止条件为字符组合与字符串长度相等。

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if len(digits) == 0: return []
        dic = {
            "1":[""],
            "2": ["a","b","c"],
            "3": ["d","e","f"],
            "4": ["g","h","i"],
            "5": ["j","k","l"],
            "6": ["m","n","o"],
            "7": ["p","q","r","s"],
            "8": ["t","u","v"],
            "9": ["w","x","y","z"],
            }
        path = ""
        res =[]
        def backtracking(n, start_index):
            nonlocal path, res
            if len(path) == n:
                res.append(path)
                return 
            for item in dic[digits[start_index]]:
                tmp = path
                path += item
                backtracking(n, start_index+1)
                path = tmp
        backtracking(len(digits), 0)
        return res
```
