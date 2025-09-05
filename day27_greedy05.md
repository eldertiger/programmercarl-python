# 贪心 part5
## [56. 合并区间](https://leetcode.cn/problems/merge-intervals/description/)

### 思路

维护一个当前存在部分重叠的所有区间的开始和结束位置，
1. 若重叠(即右边界大于等于当前index的左边界)，则扩展右区间;
2. 若不重叠，则将区间左边界推到res中，并更新起始和结束位置为下一个index的区间边界。
3. 将最后一个重叠或非重叠边界推到res中。

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort(key = lambda x:[x[0],x[1]])
        start = intervals[0][0]
        end   = intervals[0][1]
        res   = []
        for i in range(len(intervals)-1):
            if end >= intervals[i+1][0]:
                end = max(end,intervals[i+1][1])
            else:
                res.append([start, end])
                start = intervals[i+1][0]
                end = intervals[i+1][1]
        res.append([start, end])
        return res
```

## [738.单调递增的数字](https://leetcode.cn/problems/monotone-increasing-digits/)

### 思路
从右往左遍历，若前一个的数值大于后一个数值， 则将前一个数值减去1，后一个数值变为9，这是最近的单调递增数字，然后继续往前判断。

```python
class Solution:
    def monotoneIncreasingDigits(self, n: int) -> int:
        str_num = list(str(n))
        for i in range(len(str_num)-1, 0, -1):
            if int(str_num[i-1]) > int(str_num[i]):
                str_num[i-1] = str(int(str_num[i-1])-1)
                for j in range(i, len(str_num)):
                    str_num[j] = '9'

        return int(''.join(str_num))
```        

## [968.监控二叉树](https://leetcode.cn/problems/binary-tree-cameras/)

### 思路
贪心的思路是先用摄像头覆盖叶子节点的父节点，然后从下往上遍历，每隔2个节点放一个摄像头。
使用三种状态标记当前的节点：
1. 无摄像头覆盖，返回0；
2. 放了摄像头，返回1：
3. 有摄像头覆盖，返回2；

分情况讨论：
1. 对于空节点， 其应该属于有覆盖的状态，加入属于无覆盖状态，则其父节点需要放摄像头，对于叶子节点来说不成立，若是有摄像头状态，则叶子节点的父节点不需要放摄像头。
2. 若左右均为有覆盖的状态，则其会返回无覆盖状态（等待父节点添加摄像头）；
3. 若左右有一个是无覆盖，则需要添加一个摄像头，摄像头数量+1；
4. 若左右有一个是摄像头，则返回有覆盖状态；
5. 对于root节点，若左右均为有覆盖的状态，则其会返回无覆盖，但是此时没有摄像头监控root节点，需要额外增加一个摄像头。

采用后续遍历如下：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:

    def minCameraCover(self, root: Optional[TreeNode]) -> int:
        result = 0
        def dfs(node):
            nonlocal result
            if not node: return 2
            left = dfs(node.left)
            right = dfs(node.right)
            if left == 2 and right == 2:
                return 0
            elif left == 0 or right == 0:
                result += 1
                return 1
            elif left == 1 or right == 1:
                return 2
        return result + 1 if dfs(root) == 0 else result
```