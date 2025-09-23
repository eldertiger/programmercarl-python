# 单调栈 part2

## [42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)

### 暴力法
按照列方向计算雨水，找出每一列左右两侧的最大高度，最小高度和该列的高度差值即为该列可以存储的水量，复杂度为O($n^2$)。会超时。

### 单调栈
按照行方向计算雨水。
还是每个位置的下一个比其大的高度，维持一个单调递增栈(栈顶到栈底)；
1. 若当前高度大于栈顶，则说明栈顶下方的元素和当前元素构成的凹槽可以积水，面积为两个高度大小的小值和栈顶高度之差乘上两个高度对应下标之差减一的宽度。
2. 若当前高度小于等于栈顶，则无法形成可以积水的凹槽，入栈；

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        stack = [0]
        res = 0
        for i in range(1, len(height)):
            if height[i] <= height[stack[-1]]:
                stack.append(i)
            else:
                while stack and height[i] > height[stack[-1]]:
                    mid = stack.pop()
                    if stack:
                        index = stack[-1]
                        h = min(height[index],height[i])
                        if h> height[mid]:
                            res += (h-height[mid])*(i-index-1)
                stack.append(i)
        return res
```
### 双指针
使用两个数组维护当前高度的左边最大高度和右边最大高度(均包括当前高度)；
然后分别求每列的积水量：左右最大高度的小值和当前高度之差，若小于0则不能积水。
```python
class Solution:
    def trap(self, height: List[int]) -> int:
        left = [0] * len(height)
        right = [0] * len(height)
        left[0] = height[0]
        right[-1] = height[-1]
        for i in range(1,len(height)):
            left[i] = max(left[i-1],height[i])
            right[len(height)-i-1] = max(right[len(height)-i],height[len(height)-i])
        res = 0
        for i in range(len(height)):
            res += max(min(left[i],right[i])-height[i],0)
        return res
```


## [84.柱状图中最大的矩形](https://leetcode.cn/problems/largest-rectangle-in-histogram/)
### 单调栈
维护一个单调递减的栈(栈顶到栈底)：  
1. 当前高度大于栈顶元素，压入栈；
2. 当前元素等于栈顶元素，出栈再入栈，也可以跳过，重复了；
3. 当前元素小于栈顶元素，需要将大于当前元素的值全部推出，对于每个推出的元素，计算其对应的最大矩形，即其下方元素的index(左侧第一个小于其高度的元素)到当前元素index之间的以栈顶元素为高度的矩形面积。


```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        heights.insert(0, 0)
        heights.append(0)        
        stack = [0]
        res = 0 
        for i in range(1,len(heights)):
            # 情况一
            if heights[i] > heights[stack[-1]]:
                stack.append(i)
            # 情况二
            elif heights[i] == heights[stack[-1]]:
                stack.pop()
                stack.append(i)
            # 情况三
            else:
                # 抛出所有较高的柱子
                while stack and heights[i] < heights[stack[-1]]:
                    # 栈顶就是中间的柱子，主心骨
                    mid_index = stack.pop()
                    if stack:
                        left_index = stack[-1]
                        right_index = i
                        width = right_index - left_index - 1
                        height = heights[mid_index]
                        res = max(res, width * height)
                stack.append(i)
        return res

```