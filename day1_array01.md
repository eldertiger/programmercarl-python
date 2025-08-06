# 数组 part 1
## 一、 [704 二分查找](https://leetcode.cn/problems/binary-search/description/)

>给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果 target 存在返回下标，否则返回 -1。
>
>你必须编写一个具有 O(log n) 时间复杂度的算法。
### 方法1： 左闭右闭
整形可能越界，所以`m`写成 `m=i+(j-i)//2`或者`m=i+(j-i)>>1`比较好，python似乎不用考虑该问题。
1. 时间复杂度O(log n)；
2. 空间复杂度O(1)。

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        i, j = 0, len(nums)-1
        while i<=j:
            m = (i+j)//2
            if nums[m] < target:    # 目标值在[m+1,j]
                i = m+1
            elif nums[m] > target:  # 目标值在[i,m-1]
                j = m-1
            else:
                return m
        return -1
```
### 方法二：左闭右开
相比于左右闭区间`[i, j]`，开区间`[i,j)`应该保证`i<j`,且右区间的新值应该为m
1. 时间复杂度O(log n)；
2. 空间复杂度O(1)。

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        i, j = 0, len(nums)
        while i<j:
            m = (i+j)//2
            if nums[m] < target:     # 目标值在[m+1,j)
                i = m+1
            elif nums[m] > target:   # 目标值在[i,m)
                j = m
            else:
                return m
        return -1
```

## 二、 [35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/description/)
>给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
>
>请必须使用时间复杂度为 O(log n) 的算法。
### 方法：
二分查找，找到target则返回target位置`m`，否则返回插入位置`i`或`j+1`。  
### 为什么插入位置是`i`或`j+1`：
假设插入位置为`k`, 则`k`满足  
`nums[k-1]<target<nums[k]` 
当`i`,`j`跳出循环时，必定有`i=j+1`,此时`nums[j]<target<nums[i]`
1. 时间复杂度O(log n)；
2. 空间复杂度O(1)。

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        i, j = 0, len(nums)-1
        while i<=j:
            m = (i+j)//2
            if nums[m]>target:
                j = m-1
            elif nums[m]<target:
                i = m+1
            else:
                return m
        return i
```
## 三、 [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/description/)
>给你一个按照非递减顺序排列的整数数组 nums，和一个目标值 target。请你找出给定目标值在数组中的开始位置和结束位置。
>
>如果数组中不存在目标值 target，返回 [-1, -1]。
>
>你必须设计并实现时间复杂度为 O(log n) 的算法解决此问题。
### 方法
1. 二分查找找到目标值，找不到则返回[-1, -1]；
2. 分别对`[0,m]`和`[m,len(nums)-1]`寻找满足 `nums[k]==target and nums[k-1]<target`和 `nums[k]==target and nums[k+1]>target` 的上下边界k值，注意k的越界；
3. 返回上下边界。

### 复杂度分析
1. 时间复杂度O(log(n)),使用了三次二分查找；
2. 空间复杂度O(1)，使用了常数空间变量。
```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        i, j = 0, len(nums)-1
        while i<=j:
            m = (i+j)//2
            if nums[m]<target:
                i = m+1
            elif nums[m]>target:
                j = m-1
            else:
                break
        if i>j or nums[m] != target: return [-1, -1]
        # find low bound
        i, j = 0, m
        while i<=j:
            k = (i+j)//2
            if nums[k]==target:
                if k<1 or nums[k-1]<target:
                    low = k
                    break
                else:
                    j = k-1
            else:
                i = k+1
        # find high bound
        i, j = m, len(nums)-1
        while i<=j:
            k = (i+j)//2
            if nums[k]==target:
                if k+1>len(nums)-1 or nums[k+1]>target:
                    high = k
                    break
                else:
                    i = k+1
            else:
                j = k-1
        return [low,high]
```

## 四、 [27. 移除元素](https://leetcode.cn/problems/remove-element/description/)

>给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素。元素的顺序可能发生改变。然后返回 nums 中与 val 不同的元素的数量。
>
>假设 nums 中不等于 val 的元素数量为 k，要通过此题，您需要执行以下操作：
>
>更改 nums 数组，使 nums 的前 k 个元素包含不等于 val 的元素。nums 的其余元素和 nums 的大小并不重要。
>返回 k。
>用户评测：
>
>评测机将使用以下代码测试您的解决方案：
>
>>```
>>int[] nums = [...]; // 输入数组
>>int val = ...; // 要移除的值
>>int[] expectedNums = [...]; // 长度正确的预期答案。
>>                            // 它以不等于 val 的值排序。
>>
>>int k = removeElement(nums, val); // 调用你的实现
>>
>>assert k == expectedNums.length;
>>sort(nums, 0, k); // 排序 nums 的前 k 个元素
>>for (int i = 0; i < actualLength; i++) {
>>    assert nums[i] == expectedNums[i];
>>}
>>```
>如果所有的断言都通过，你的解决方案将会 通过。
### 方法： 双指针
`i`,`j`分别指向开始和结束，每当遇到和`val`相等的值，则将`i+1,i+1,...,j`的值赋值给`i,i+1,...,j-1`, 执行`j -= 1`，否则，`i += 1`
1. 时间复杂度O(n),最多比较n次，没有val；
2. 空间复杂度O(1)。
```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        i, j = 0, len(nums)-1
        while i<=j:
            if nums[i] == val:
                nums[i:j]= nums[i+1:j+1]
                j -= 1
            else:
                i += 1
        return i
```

## 五、 [977. 有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/description/)

>给你一个按 非递减顺序 排序的整数数组 nums，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。

### 方法：
依次比较首尾两端的值的平方大小，将大值入栈，最后将栈逆序输即可。  
1. 时间复杂度O(n),一共比较了n次；
2. 空间复杂度O(n)，使用了额外空间的列表。
```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        i, j = 0, len(nums)-1
        res = []
        while i<=j:
            if nums[i]*nums[i]<nums[j]*nums[j]:
                res.append(nums[j]*nums[j])
                j -=1
            else:
                res.append(nums[i]*nums[i])
                i += 1
        return res[::-1]
```