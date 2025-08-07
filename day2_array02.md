# # 数组 part 2
## 一、 [209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/description/)
>给定一个含有 n 个正整数的数组和一个正整数 target 。
>
>找出该数组中满足其总和大于等于 target 的长度最小的 子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。

 
### 方法：快慢指针
慢指针更新指向满足子数组小于target的位置，然后再更新快指针，找到大于target的位置；  
时间复杂度O(n)，空间复杂度O(1)  
题目还要求实现O(n log n)的复杂度，大概是先排序，再用二分法查找。

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        cur_sum = 0
        min_len = float(inf)
        slow, fast = 0, 0
        while fast < len(nums):
            cur_sum += nums[fast]
            while cur_sum >= target:
                min_len = min(min_len, fast - slow + 1)
                cur_sum -= nums[slow]
                slow += 1
            fast += 1
        return min_len if min_len != float(inf) else 0
```

## 二、 [59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/description/)

>给你一个正整数 n ，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的 n x n 正方形矩阵 matrix 。

### 方法：更新边界
采用l, r, t, b作为左右上下的边界，模拟数字的增大，当增大到右边界，则对应上边界减1，依次循环，直到n*n。  
1. 时间复杂度$O(n^2)$；
2. 空间复杂度$O(n^2)$  

Python 中 可变对象 (mutable object) 和 不可变对象 (immutable object) 的概念：  
注意matrix的初始化不能使用`matrix = [[0]*n]*n`，列表 (list) 是 可变对象，这意味着它们的内容可以在创建后被修改，使用 * 运算符复制一个包含可变对象的列表时，它只复制了对该对象的引用，而不是对象本身。  
在绝大多数需要创建二维数组的情况下，都应该使用 [[0 for _ in range(n)] for _ in range(n)] 的方式来确保每一行都是独立的，从而避免意外的副作用。
```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        l, r, t, b = 0, n-1, 0, n-1
        num = 1
        target = n*n
        matrix = [[0 for _ in range(n)] for _ in range(n)]
        while num <= target:
            for i in range(l, r+1):
                matrix[t][i] = num
                num += 1
            t += 1
            for i in range(t, b+1):
                matrix[i][r] = num
                num += 1
            r -= 1
            for i in range(r, l-1, -1):
                matrix[b][i] = num
                num += 1
            b -= 1
            for i in range(b, t-1, -1):
                matrix[i][l] = num
                num += 1
            l += 1
        return matrix
```

## 三、 58. [区间和](https://www.programmercarl.com/kamacoder/0058.%E5%8C%BA%E9%97%B4%E5%92%8C.html)

>题目描述
>
>给定一个整数数组 Array，请计算该数组在每个指定区间内元素的总和。
>
>输入描述
>
>第一行输入为整数数组 Array 的长度 n，接下来 n 行，每行一个整数，表示数组的元素。随后的输入为需要计算总和的区间，直至文件结束。
>
>输出描述
>
>输出每个指定区间内元素的总和。

本题考查ACM的输入输出。  
python采用sys.stdin.read() 会一次性读取标准输入为字符串, Python 内置的 input() 函数一次只读取一行。
直接计算累加和，需要计算某个区间则用区间右侧边界的累加和值减去区间左侧边界左边的累加和。
1. 时间复杂度：$O(n)$
2. 空间复杂度: $O(n)$

```python
import sys
input = sys.stdin.read
def main():
    data = input().split()
    index = 0
    n = int(data[index])
    index += 1
    vec = []
    p = [0]*n
    presum = 0
    for i in range(n):
        presum += int(data[index+i])
        p[i] = presum
    index += n

    results = []
    while index < len(data):
        a = int(data[index])
        b = int(data[index+1])
        index += 2
        if a == 0:
            sum_value = p[b]
        else:
            sum_value = p[b] - p[a-1]
        results.append(sum_value)
    
    for result in results:
        print(result)
if __name__ == "__main__":
    main()
```

## 四、 [44. 开发商购买土地](https://www.programmercarl.com/kamacoder/0044.%E5%BC%80%E5%8F%91%E5%95%86%E8%B4%AD%E4%B9%B0%E5%9C%9F%E5%9C%B0.html)
>在一个城市区域内，被划分成了n * m个连续的区块，每个区块都拥有不同的权值，代表着其土地价值。目前，有两家开发公司，A 公司和 B 公司，希望购买这个城市区域的土地。
>
>现在，需要将这个城市区域的所有区块分配给 A 公司和 B 公司。
>
>然而，由于城市规划的限制，只允许将区域按横向或纵向划分成两个子区域，而且每个子区域都必须包含一个或多个区块。
>
>为了确保公平竞争，你需要找到一种分配方式，使得 A 公司和 B 公司各自的子区域内的土地总价值之差最小。
>
>注意：区块不可再分。

输入描述
>第一行输入两个正整数，代表 n 和 m。
>
>接下来的 n 行，每行输出 m 个正整数。
>
>输出描述
>
>请输出一个整数，代表两个子区域内土地总价值之间的最小差距。

### 方法：
根据第三题前缀和的思路，直接统计横向和纵向所有值的前缀和的和，再分别对横向和纵向进行循环，比较区间值的大小，得到最小结果对应的分类。
1. 时间复杂度$O(n*m)$
2. 空间复杂度$O(n*m)$
主要注意点在于data的index的大小变化
```python
import sys
input = sys.stdin.read
def main():
    data = input().split()
    n, m = int(data[0]), int(data[1])
    horizontal = [0]*m
    vertical = [0]*n
    index = 2
    # 统计水平和垂直前缀和
    for i in range(n):
        for j in range(m):
            horizontal[j] += int(data[index+i*m+j])
            vertical[i] += int(data[index+i*m+j])
    for j in range(1,m):
        horizontal[j] += horizontal[j-1]
    for i in range(1,n):
        vertical[i] += vertical[i-1]
    # 计算分割差异最小值，可以用二分法加快速度
    min_diff = max(horizontal[m-1],vertical[n-1])
    for j in range(1,m):
        min_diff = min(min_diff,abs(horizontal[m-1] - 2*horizontal[j-1]))
    for i in range(1,n):
        min_diff = min(min_diff,abs(vertical[n-1] - 2*vertical[i-1]))
    print(min_diff)
if __name__ == "__main__":
    main()

```