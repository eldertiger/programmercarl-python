# 贪心 part4

## [452. 用最少数量的箭引爆气球](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/description/)

### 思路

本题是寻找部分重叠在一起的气球；
首先需要排序，对起始位置排序，从左往右遍历，若相邻不重叠，则需要增加一只箭；若重叠，则需要更新重叠的边界，赋值给第二个值的右边界，继续比较更新，若下次不重叠，则这对重叠的需要增加一只箭，以此类推。箭需要额外多一个，用于最后一个组重叠的或者单个气球。

```python
class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        points.sort(key = lambda x:(x[0],x[1]) )
        count = 1
        for i in range(len(points)-1):
            if points[i][1] < points[i+1][0]:
                count +=1
            else:
                points[i+1][1] = min(points[i][1],points[i+1][1])
        return count
```

## [435. 无重叠区间](https://leetcode.cn/problems/non-overlapping-intervals/description/)

### 思路

计算出不重叠的有多少个区间，然后用总数减去不重叠的区间就是需要移除的区间数量，和上一题一样，最后用总数减去射箭的数量即可,只不过这里左右边界相等也是不重合的。

```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        intervals.sort(key = lambda x:(x[0],x[1]))
        count = 1
        for i in range(len(intervals)-1):
            if intervals[i][1] <= intervals[i+1][0]:
                count += 1
            else:
                intervals[i+1][1] = min(intervals[i][1], intervals[i+1][1])
        return len(intervals)-count
```

## [763.划分字母区间](https://leetcode.cn/problems/partition-labels/)

### 思路

首先要确定每个字符在字符串中出现的位置区间，实际上只需要知道最远区间就可以了；  
然后从左往右遍历，维护一个当前遍历的区间的开始位置，以及遍历过的字符的最远位置，若遍历到该最远位置，则可以切割，更新开始位置到下一个index。

```python
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        dic = defaultdict()
        for i in range(len(s)):
            dic[s[i]] = i
        res = []
        start = 0
        end = 0
        for i in range(len(s)):
            end = max(dic[s[i]], end)
            if i == end:
                res.append(i+1-start)
                start = i+1
        return res
```