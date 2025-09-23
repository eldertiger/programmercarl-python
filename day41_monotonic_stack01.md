# 单调栈 part1
## [739. 每日温度](https://leetcode.cn/problems/daily-temperatures/)

### 思路
单调栈，维护一个数组下标的栈，新元素和栈顶下标元素比较：
1. 若新元素小于等于栈顶下标元素，则将新元素加入栈，因为没有找到大于栈顶元素的温度；
2. 若新元素大于栈顶下标元素，则将栈顶元素退出，同时记录对应下标和新元素下标的距离，继续比较直到栈为空或栈顶元素大于等于新元素；将新元素下标压入栈。

初始化根据题设都是0。

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        answer = [0]*len(temperatures)
        stack = [0]
        for i in range(1,len(temperatures)):
            if temperatures[i] <= temperatures[stack[-1]]:
                stack.append(i)
            else:
                while stack and temperatures[i] > temperatures[stack[-1]]:
                    index = stack.pop()
                    answer[index] = i-index
                stack.append(i)
                
        return answer

```

## [496.下一个更大元素 I](https://leetcode.cn/problems/next-greater-element-i/)

### 思路
和上题一样，我们对nums2中的每个元素寻找大于其的下一个元素，若该元素在nums1中，则将下一个元素加入结果中对应的nums1的index里。

```python
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        ans = [-1] * len(nums1)
        stack = [0]

        for i in range(1,len(nums2)):
            if nums2[i] <= nums2[stack[-1]]:
                stack.append(i)
            else:
                while stack and nums2[i] > nums2[stack[-1]]:
                    if nums2[stack[-1]] in nums1:
                        index = nums1.index(nums2[stack[-1]])
                        ans[index]=nums2[i]
                    stack.pop()
                stack.append(i)
        return ans
```

## [503.下一个更大元素II](https://leetcode.cn/problems/next-greater-element-ii/description/)
### 思路
和每日温度一样的思路，只不过多循环一遍nums。

```python
class Solution:
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        ans = [-1] * len(nums)
        stack = [0]
        for i in range(1,len(nums)*2):
            i = i%len(nums) if i>=len(nums) else i
            if nums[i] <= nums[stack[-1]]:
                stack.append(i)
            else:
                while stack and nums[i]> nums[stack[-1]]:
                    index = stack.pop()
                    ans[index] = nums[i]
                stack.append(i)
        return ans            
```

