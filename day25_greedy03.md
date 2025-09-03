# 贪心 part3

## [134. 加油站](https://leetcode.cn/problems/gas-station/description/)

### 思路
1. 如果gas总量小于cost， 则无论如何也跑不到终点。
2. 维持一个最小的油箱容量，若容量大于等于0，说明从起点到终点，一直有油，返回起点即可；
3. 从后往前遍历，直到最小的负油箱容量被填平，说明从此处开始往后，可以满足一直跑回来；(题设一定有解)

```python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        cur_sum = 0
        min_fuel = float("inf")
        for i in range(len(gas)):
            cur_sum += gas[i] - cost[i]
            if cur_sum < min_fuel:
                min_fuel = cur_sum
        if cur_sum < 0: return -1
        if min_fuel >=0: return 0
        for i in range(len(gas)-1, -1, -1):
            min_fuel += gas[i] - cost[i]
            if min_fuel>=0: return i
```

### 思路2
若当前油箱累积和为负数，则说明从此处开始，无法到达下一个点，那么将起点更新为下一个点，重置当前累积和；
若某个点为最终的起点，则从其开始的累计和最终应该大于等于0；

```python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        cur_sum = 0
        total_sum = 0
        start = 0
        min_fuel = float("inf")
        for i in range(len(gas)):
            cur_sum += gas[i] - cost[i]
            total_sum += gas[i]-cost[i]
            if cur_sum <0:
                start = i+1
                cur_sum =0
        if total_sum < 0: return -1
        return start
```

## [135. 分发糖果](https://leetcode.cn/problems/candy/)

## 思路
从左往右遍历，所有大于左侧孩子分数的孩子比左侧多得一个糖果；
再从右往左遍历，所有大于右侧孩子分数的孩子若和右侧一样多，则比右侧多得一个糖果，否则等于其本身(不可能比右侧小)；
这样左右两侧的条件就都满足了。

```python
class Solution:
    def candy(self, ratings: List[int]) -> int:
        res = [1] * len(ratings)
        for i in range(1, len(ratings)):
            if ratings[i]>ratings[i-1]:
                res[i] = res[i-1]+1
        for i in range(len(ratings)-1, 0, -1):
            if ratings[i-1]>ratings[i]:
                res[i-1] = max(res[i]+1, res[i-1])
        return sum(res)
```

## [860.柠檬水找零](https://leetcode.cn/problems/lemonade-change/description/)

### 思路
使用哈希字典存储当前的余额钱币种类和数量，  
1. 顾客付10块时，只能找5块；
2. 付20块时，若10块没钱，则找3张5块，若10块有钱，则优先找一张10块加一张5块，
3. 每次付完判断5块或者10块数量是否小于0，小于0则说明无法找零。

记录20其实没有意义，因为不能用于找零。
```python
class Solution:
    def lemonadeChange(self, bills: List[int]) -> bool:
        dic = defaultdict(int)
        for i in range(len(bills)):
            dic[bills[i]] += 1
            if bills[i] == 10:
                if dic[5] > 0:
                    dic[5] -= 1
                else:
                    return False
                
            if bills[i] == 20:
                if dic[5]>=3 and dic[10]==0:
                    dic [5] -= 3
                elif dic[5] >=1 and dic[10]>=1:
                    dic[10] -= 1
                    dic[5] -= 1
                else:
                    return False
        return True        
```

## [406.根据身高重建队列](https://leetcode.cn/problems/queue-reconstruction-by-height/description/)

### 思路
先按照身高顺序对所有人进行排序，由于默认前面的都大于等于自己身高，只需要按照自己的排序插到对应位置即可满足前面k个人大于等于自己的身高。

```python
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        people.sort(key=lambda x: (-x[0], x[1]))
        que = []
        for p in people:
            que.insert(p[1], p)
        return que
```

