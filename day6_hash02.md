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

## 三、 [15. 三数之和](https://leetcode.cn/problems/3sum/description/)

### 双指针法：

首先对数组排序，然后对于每个元素，使用双指针指向下一个元素和最后一个元素，判断三者之和与0的关系，大于0，则缩小右指针，小于0，则增大左指针，等于0， 则添加三个元素数组到结果中，跳过左右两边相等的元素，并同时缩小区间。  
此外还要跳过与之前元素相同的元素以及大于0的元素。
1. 时间复杂度$O(n^2)$，遍历加双指针，排序只有$O(n log n)$
2. 空间复杂度$O(1)$，使用常数空间。
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        res = []
        for i in range(len(nums)-2):
            if nums[i] > 0:
                break
            if i > 0 and nums[i] == nums[i-1]:
                continue

            left, right = i+1, len(nums)-1
        
            while left < right:
                if nums[i] + nums[left]+nums[right] > 0:
                    right -= 1
                elif nums[i] + nums[left]+nums[right] < 0:
                    left += 1
                else:
                    res.append([nums[i],nums[left],nums[right]])
                    # 跳过相同的元素
                    while right > left and nums[right] == nums[right - 1]:
                        right -= 1
                    while right > left and nums[left] == nums[left + 1]:
                        left += 1
                        
                    right -= 1
                    left += 1
        return res
```

## 四、 [18. 四数之和](https://leetcode.cn/problems/4sum/description/)

### 方法：
在三数之和的基础上多增加一道循环，同时额外对两数之和进行剪枝条。
除了$nums[i]>target$,还需要nums[i]>=0,因为只有正数才能够满足条件。
```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()
        res = []
        for i in range(len(nums)):
            if nums[i] > target and nums[i] >= 0:
                break
            if i>0 and nums[i] == nums[i-1]:
                continue
            for j in range(i+1, len(nums)-2):
                if nums[i] + nums[j] > target and target >= 0: 
                    break
                if j > i+1 and nums[j] == nums[j-1]: # 去重
                    continue
                left, right = j+1, len(nums)-1
                while left < right:
                    sum_ = nums[i]+nums[j]+nums[left]+nums[right]
                    if sum_ > target:
                        right -= 1
                    elif sum_ < target:
                        left += 1
                    else:
                        res.append([nums[i],nums[j],nums[left],nums[right]])
                        while right > 1 and nums[right] == nums[right-1]:
                            right -= 1
                        while left <= len(nums)-2 and nums[left] == nums[left+1]:
                            left += 1
                        right -= 1
                        left += 1
        return res
```