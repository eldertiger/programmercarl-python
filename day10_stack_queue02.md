# 栈与队列part2

## 一、 [150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/description/)
### 方法：
逆波兰表达式采用堆栈实现，当遇到操作数时入栈，当遇到操作符时出栈操作符需要数量的操作数，进行计算后重新入栈，本题只有基本的操作符，采用if分类实现。  
由于所有输入都是合法，就没有判断stack剩余操作数数量了。
1. 时间复杂度$O(n)$;
2. 空间复杂度$O(n)$
```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        for i in range(len(tokens)):
            if tokens[i] == "+":
                a = stack.pop()
                b = stack.pop()
                stack.append(b+a)
            elif tokens[i] == "-":
                a = stack.pop()
                b = stack.pop()
                stack.append(b-a)
            elif tokens[i] == "*":
                a = stack.pop()
                b = stack.pop()
                stack.append(b*a)
            elif tokens[i] == "/":
                a = stack.pop()
                b = stack.pop()
                stack.append(int(b/a))
            else:
                stack.append(int(tokens[i]))
        return stack[-1]
```

简化a,b 如下：注意stack.pop()的顺序。也可以使用map生成操作符到函数的映射。
```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        for i in range(len(tokens)):
            if tokens[i] == "+":
                stack.append(stack.pop()+stack.pop())
            elif tokens[i] == "-":
                stack.append(-stack.pop()+stack.pop())
            elif tokens[i] == "*":
                stack.append(stack.pop()*stack.pop())
            elif tokens[i] == "/":
                stack.append(int(1/stack.pop()*stack.pop()))
            else:
                stack.append(int(tokens[i]))
        return stack[-1]
```

## 二、 [239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/description/)
### 方法：
暴力法采用对每个窗口求最大值，其时间复杂度为$O(nk)$;  
维持一个窗口中的最大值和可能成为最大值的队列是解题关键，这样可以将求最大值的时间复杂度压缩到$O(1)$;  
1. 初始化第一个窗口队列，队列按照原有顺序保持递减，即只保留比队列末尾大的数值，将队尾出队，新值入队。这样队首总是保持最大值。将队首最大值入栈到结果中。
2. 对于每一个新入队`i`和出队`i-k`窗口的值，判断出队值是否是最大值，是的则将其出队；判断入队值与队尾的关系，按照1中的步骤入队。将队首最大值入栈到结果中。

本题目重点在于单调队列的建立用于维持队首最大值。

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        if not nums or k == 0: return []
        deque = collections.deque()
        # 形成第一个递减队列窗口
        for i in range(k):
            while deque and deque[-1] < nums[i]:
                deque.pop()
            deque.append(nums[i])
        res = [deque[0]]
        # 形成窗口后
        for i in range(k, len(nums)):
            if deque[0] == nums[i - k]:
                deque.popleft()
            while deque and deque[-1] < nums[i]:
                deque.pop()
            deque.append(nums[i])
            res.append(deque[0])
        return res
```

## 三、 [347.前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/description/)

### 方法：
暴力法首先遍历整个数组，并使用哈希表记录每个数字出现的次数，并形成一个「出现次数数组」。找出原数组的前 k 个高频元素，就相当于找出「出现次数数组」的前 k 大的值。最简单的做法是给「出现次数数组」排序。但由于可能有 O(N) 个不同的出现次数（其中 N 为原数组长度），故总的算法复杂度会达到 O(NlogN)，不满足题目的要求。  

所以使用优先级队列(小顶堆)来实现维护较大的k个频次元素。  
依次将排好序的value-key(频次)压入小顶堆，维持k个频次较大的元素，超过k个每次将最小的元素从小顶堆删除。  
然后逆序输出小顶堆，即为频次从大到小的元素排列。
```python
import heapq
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        dic = defaultdict(int)
        for i in range(len(nums)):
            dic[nums[i]] += 1
        pri_que = [] #小顶堆
        
        #用固定大小为k的小顶堆，扫描所有频率的数值
        for key, freq in dic.items():
            heapq.heappush(pri_que, (freq, key))
            if len(pri_que) > k: #如果堆的大小大于了K，则队列弹出，保证堆的大小一直为k
                heapq.heappop(pri_que)
        
        #找出前K个高频元素，因为小顶堆先弹出的是最小的，所以倒序来输出到数组
        result = [0] * k
        for i in range(k-1, -1, -1):
            result[i] = heapq.heappop(pri_que)[1]
        return result
```
### 方法2： 频次-元素列表
统计好频次后，按照频次，统计同一频次的所有元素，然后对频次排序，从大到小输出元素到结果中，直到达到k个元素。
```python
import heapq
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        dic = defaultdict(int)
        for i in range(len(nums)):
            dic[nums[i]] += 1
        index_dict = defaultdict(list)
        for key in dic:
            index_dict[dic[key]].append(key)
        keys = list(index_dict.keys()) 
        keys.sort()
        result = []
        cnt = 0
        while keys and cnt <k:
            result += index_dict[keys[-1]]
            cnt += len(index_dict[keys[-1]])
            keys.pop()
        return result
```