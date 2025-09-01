# 回溯 part4

## [491.递增子序列](https://leetcode.cn/problems/non-decreasing-subsequences/)

### 递归法

在不排序的情况下，如何对同一层遍历进行去重，可以使用set来去重；除了去重，还需要跳过所有不满足递增条件的元素。其余按照正常的回溯流程走即可。将长度大于1的path加入res中。

```python
class Solution:
    def __init__(self):
        self.res = []
        self.path = []
    def backtracking(self, nums, start_index):
        if (len(self.path) > 1):
            self.res.append(self.path[:])     
        uset = set() # 通过set去除使用过的元素
        for i in range(start_index, len(nums)):
            if (self.path and nums[i] < self.path[-1]) or nums[i] in uset:
                continue
            uset.add(nums[i])
            self.path.append(nums[i])
            self.backtracking(nums, i+1)
            self.path.pop()  
    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        self.backtracking(nums,0)
        return self.res
```

## [46.全排列](https://leetcode.cn/problems/permutations/description/)

### 递归法

全排列就是每次遍历全部元素，但是要去除path中已有的元素，其他和回溯法相同。

```python
class Solution:
    def __init__(self):
        self.res = []
        self.path = []
    def backtracking(self,nums):
        if len(self.path) == len(nums):
            self.res.append(self.path[:])
            return
        for i in range(len(nums)):
            if nums[i] in self.path:
                continue
            self.path.append(nums[i])
            self.backtracking(nums)
            self.path.pop()
    def permute(self, nums: List[int]) -> List[List[int]]:
        self.backtracking(nums)
        return self.res
```

使用数组判断是否使用过，数组哈希更快一点

```python
class Solution:
    def __init__(self):
        self.res = []
        self.path = []
    def backtracking(self,nums,used):
        if len(self.path) == len(nums):
            self.res.append(self.path[:])
            return
        for i in range(len(nums)):
            if used[i] == 0:
                used[i] = 1
                self.path.append(nums[i])
                self.backtracking(nums, used)
                self.path.pop()
                used[i] = 0
    def permute(self, nums: List[int]) -> List[List[int]]:
        used = [0]*len(nums)
        self.backtracking(nums, used)
        return self.res
```


## [47.全排列 II](https://leetcode.cn/problems/permutations-ii/description/)

### 递归法

相比于全排列，需要先对数组进行排序，然后增加判断同一层不会重复使用相等的元素，从而避免重复的排列。方法类似组合总和2和子集2。

这道题后续需要回顾一下。

```python
class Solution:
    def __init__(self):
        self.path = []
        self.res = []
    def backtracking(self, nums, used):
        if len(self.path) == len(nums):
            self.res.append(self.path[:])
            return
        for i in range(len(nums)):
            if i > 0 and nums[i] == nums[i-1] and used[i-1]==0:
                continue
            if used[i] == 0:
                self.path.append(nums[i])
                used[i] = 1
                self.backtracking(nums,used)
                self.path.pop()
                used[i] = 0

    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        used = [0]*len(nums)
        self.backtracking(nums,used)
        return self.res
```

## [51. N皇后](https://leetcode.cn/problems/n-queens/description/)

### 方法
本体思路：

将棋盘想象成长度为n的数组和需要组成的长度为n的组合，每次操作时，需要满足N皇后的条件；  
递归的推出条件为row遍历到了n；  
注意每行的字符串复制应采用拼接的方式，没办法使用矩阵index直接赋值。  

需要重刷一遍。

```python
class Solution:
    def __init__(self):
        self.res = []
    def isValid(self, row, col, chessboard, n):
        for i in range(row):
            if chessboard[i][col] == "Q": 
                return False
        i, j = row-1, col-1
        while i>=0 and j>=0:
            if chessboard[i][j] == "Q":
                return False
            i -= 1
            j -= 1
        i, j = row-1, col + 1
        while i >= 0 and j < n:
            if chessboard[i][j] == "Q":
                return False
            i -= 1
            j += 1
        return True


    def backtracking(self, n, row, chessboard):
        if row == n:
            self.res.append(chessboard[:])
            return
        for col in range(n):
            if self.isValid(row, col, chessboard, n):
                chessboard[row] = chessboard[row][:col] + "Q" + chessboard[row][col+1:]
                self.backtracking(n, row+1, chessboard)
                chessboard[row] = chessboard[row][:col] + "." + chessboard[row][col+1:]


    def solveNQueens(self, n: int) -> List[List[str]]:
        chessboard = ['.'*n for _ in range(n)]
        self.backtracking(n, 0, chessboard)
        return [[''.join(row) for row in solution] for solution in self.res] 
```

## [37. 解数独](https://leetcode.cn/problems/sudoku-solver/description/)

### 方法：

如下代码会超时, 可能也有bug：


```python
class Solution:
    def isValid(self, board, row, col, num):
        for i in range(len(board)):
            if board[i][col] == num:
                return False
        for j in range(len(board[0])):
            if board[row][j] == num:
                return False
        start_row = row//3*3
        start_col = col//3*3
        for i in range(start_row, start_row+3):
            for j in range(start_col, start_col+3):
                if board[i][j] == num:
                    return False
        return True

    def backtracking(self, board):
        for i in range(len(board)):
            for j in range(len(board[0])):
                if board[i][j] == ".":
                    for num in range(1,10):
                        if self.isValid(board,i,j,str(num)):
                            board[i][j] = str(num)
                            if self.backtracking(board): return True
                            board[i][j] = "."
                else:
                    continue
                return False
        return True
                
    def solveSudoku(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        self.backtracking(board)
```

改进，做剪枝，删去无法使用的num：

```python
class Solution:
    def solveSudoku(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        row_used = [set() for _ in range(9)]
        col_used = [set() for _ in range(9)]
        box_used = [set() for _ in range(9)]
        for row in range(9):
            for col in range(9):
                num = board[row][col]
                if num == ".":
                    continue
                row_used[row].add(num)
                col_used[col].add(num)
                box_used[(row // 3) * 3 + col // 3].add(num)
        self.backtracking(0, 0, board, row_used, col_used, box_used)

    def backtracking(
        self,
        row: int,
        col: int,
        board: List[List[str]],
        row_used: List[List[int]],
        col_used: List[List[int]],
        box_used: List[List[int]],
    ) -> bool:
        if row == 9:
            return True

        next_row, next_col = (row, col + 1) if col < 8 else (row + 1, 0)
        if board[row][col] != ".":
            return self.backtracking(
                next_row, next_col, board, row_used, col_used, box_used
            )

        for num in map(str, range(1, 10)):
            if (
                num not in row_used[row]
                and num not in col_used[col]
                and num not in box_used[(row // 3) * 3 + col // 3]
            ):
                board[row][col] = num
                row_used[row].add(num)
                col_used[col].add(num)
                box_used[(row // 3) * 3 + col // 3].add(num)
                if self.backtracking(
                    next_row, next_col, board, row_used, col_used, box_used
                ):
                    return True
                board[row][col] = "."
                row_used[row].remove(num)
                col_used[col].remove(num)
                box_used[(row // 3) * 3 + col // 3].remove(num)
        return False
```