# 图论 part10
## [Bellman_ford 队列优化算法（又名SPFA）](https://www.programmercarl.com/kamacoder/0094.%E5%9F%8E%E5%B8%82%E9%97%B4%E8%B4%A7%E7%89%A9%E8%BF%90%E8%BE%93I-SPFA.html)

### 思路
 Bellman_ford 算法每次松弛 都是对所有边进行松弛。但真正有效的松弛，是基于已经计算过的节点在做的松弛。  
只需要对 上一次松弛的时候更新过的节点作为出发节点所连接的边 进行松弛就够了。  
使用队列将每次更新过的目标节点加入队列，下一次只对更新过的目标节点的目标节点进行更新。

```python

from collections import deque
n, m = map(int, input().split())
edges = [[] for _ in range(n+1)]
for _ in range(m):
    src, dest, weight = map(int, input().split())
    edges[src].append([dest, weight])

minDist = [float('inf')]*(n+1)
minDist[1] = 0# 起点距离为0
q = deque([1])
visited = [False]*(n+1)
visited[1] = True
while q:
    cur = q.popleft()
    visited[cur] = False
    for dest, weight in edges[cur]:
        if minDist[cur] != float("inf") and minDist[cur] + weight < minDist[dest]:
            minDist[dest] = minDist[cur] + weight
            if visited[dest] == False:
                q.append(dest)
                visited[dest] = True
if minDist[-1] == float("inf"):
    print("unconnected")
else:
    print(minDist[-1])

```

## [bellman_ford之判断负权回路](https://www.programmercarl.com/kamacoder/0095.%E5%9F%8E%E5%B8%82%E9%97%B4%E8%B4%A7%E7%89%A9%E8%BF%90%E8%BE%93II.html#%E6%80%9D%E8%B7%AF)

卡码网：95. 城市间货物运输 II(opens new window)

### 【题目描述】

某国为促进城市间经济交流，决定对货物运输提供补贴。共有 n 个编号为 1 到 n 的城市，通过道路网络连接，网络中的道路仅允许从某个城市单向通行到另一个城市，不能反向通行。

网络中的道路都有各自的运输成本和政府补贴，道路的权值计算方式为：运输成本 - 政府补贴。权值为正表示扣除了政府补贴后运输货物仍需支付的费用；

权值为负则表示政府的补贴超过了支出的运输成本，实际表现为运输过程中还能赚取一定的收益。

然而，在评估从城市 1 到城市 n 的所有可能路径中综合政府补贴后的最低运输成本时，存在一种情况：图中可能出现负权回路。

负权回路是指一系列道路的总权值为负，这样的回路使得通过反复经过回路中的道路，理论上可以无限地减少总成本或无限地增加总收益。

为了避免货物运输商采用负权回路这种情况无限的赚取政府补贴，算法还需检测这种特殊情况。

请找出从城市 1 到城市 n 的所有可能路径中，综合政府补贴后的最低运输成本。同时能够检测并适当处理负权回路的存在。

城市 1 到城市 n 之间可能会出现没有路径的情况

### 【输入描述】

第一行包含两个正整数，第一个正整数 n 表示该国一共有 n 个城市，第二个整数 m 表示这些城市中共有 m 条道路。

接下来为 m 行，每行包括三个整数，s、t 和 v，表示 s 号城市运输货物到达 t 号城市，道路权值为 v。

### 【输出描述】

如果没有发现负权回路，则输出一个整数，表示从城市 1 到城市 n 的最低运输成本（包括政府补贴）。

如果该整数是负数，则表示实现了盈利。如果发现了负权回路的存在，则输出 "circle"。如果从城市 1 无法到达城市 n，则输出 "unconnected"。

### 输入示例

4 4
1 2 -1
2 3 1
3 1 -1
3 4 1
### 输出示例

circle

### 思路
需要判断负权回路和无连接。
负权回路意味着更新n次时相比n-1次，minDist数组还会发生变化；  
无连接就是最终终点还是最大权值。

```python
n, m = map(int, input().split())
edges = []
for _ in range(m):
    src, dest, weight = map(int, input().split())
    edges.append([src,dest, weight])

minDist = [float('inf')]*(n+1)
minDist[1] = 0# 起点距离为0
flag = False
for i in range(1,n+1):
    for src, dest, weight in edges:
        if i<n:
            if minDist[src] != float("inf") and minDist[src] + weight < minDist[dest]:
                minDist[dest] = minDist[src] + weight
        else:
            if minDist[src] != float("inf") and minDist[src] + weight < minDist[dest]:
                flag = True

if flag: 
    print("circle")
elif minDist[-1] == float("inf"):
    print("unconnected")
else:
    print(minDist[-1])

```


## [bellman_ford之单源有限最短路](https://www.programmercarl.com/kamacoder/0096.%E5%9F%8E%E5%B8%82%E9%97%B4%E8%B4%A7%E7%89%A9%E8%BF%90%E8%BE%93III.html)

### 【题目描述】

某国为促进城市间经济交流，决定对货物运输提供补贴。共有 n 个编号为 1 到 n 的城市，通过道路网络连接，网络中的道路仅允许从某个城市单向通行到另一个城市，不能反向通行。

网络中的道路都有各自的运输成本和政府补贴，道路的权值计算方式为：运输成本 - 政府补贴。

权值为正表示扣除了政府补贴后运输货物仍需支付的费用；

权值为负则表示政府的补贴超过了支出的运输成本，实际表现为运输过程中还能赚取一定的收益。

请计算在最多经过 k 个城市的条件下，从城市 src 到城市 dst 的最低运输成本。

### 【输入描述】

第一行包含两个正整数，第一个正整数 n 表示该国一共有 n 个城市，第二个整数 m 表示这些城市中共有 m 条道路。

接下来为 m 行，每行包括三个整数，s、t 和 v，表示 s 号城市运输货物到达 t 号城市，道路权值为 v。

最后一行包含三个正整数，src、dst、和 k，src 和 dst 为城市编号，从 src 到 dst 经过的城市数量限制。

### 【输出描述】

输出一个整数，表示从城市 src 到城市 dst 的最低运输成本，如果无法在给定经过城市数量限制下找到从 src 到 dst 的路径，则输出 "unreachable"，表示不存在符合条件的运输方案。

### 输入示例：
```
6 7
1 2 1
2 4 -3
2 5 2
1 3 5
3 5 1
4 6 4
5 6 -2
2 6 1
```
### 输出示例：
```
0
```
### 思路

最多经过k个城市，则需要更新k+1条边，即bellman ford算法要更新k+1次，每次应该用当前的目标节点权值和上一次的起点权值+边的权值比较进行更新，否则由于某些边按顺序排列，会在一次更新中更新。同样没有更新的话，跳出即可。


```python
n, m = map(int, input().split())
edges = []
for _ in range(m):
    src, dest, weight = map(int, input().split())
    edges.append([src,dest, weight])
start, end, k = map(int, input().split())

minDist = [float('inf')]*(n+1)
minDist[start] = 0# 起点距离为0

for i in range(k+1):
    updated = False
    minDist_copy = minDist.copy()
    for src, dest, weight in edges:
        if minDist_copy[src] != float("inf") and minDist_copy[src] + weight < minDist[dest]:
            minDist[dest] = minDist_copy[src] + weight
            updated = True
    if not updated:
        break
if minDist[end] == float("inf"):
    print("unreachable")
else:
    print(minDist[end])
```

SPFA方法优化版：
```python

def main():
    n, m = [int(i) for i in input().split()]
    graph = [[] for _ in range(n+1)]
    for _ in range(m):
        v1, v2, val = [int(i) for i in input().split()]
        graph[v1].append([v2, val])
    src, dst, k = [int(i) for i in input().split()]
    min_dist = [inf for _ in range(n+1)]
    min_dist[src] = 0  # 初始化起点的距离
    que = deque([src])

    while k != -1 and que:
        visited = [False for _ in range(n+1)]  # 用于保证每次松弛时一个节点最多加入队列一次
        que_size = len(que)
        temp_dist = min_dist.copy()  # 用于记录上一次遍历的结果
        for _ in range(que_size):
            cur_node = que.popleft()
            for next_node, val in graph[cur_node]:
                if min_dist[next_node] > temp_dist[cur_node] + val:
                    min_dist[next_node] = temp_dist[cur_node] + val
                    if not visited[next_node]:
                        que.append(next_node)
                        visited[next_node] = True
        k -= 1

    if min_dist[dst] == inf:
        print("unreachable")
    else:
        print(min_dist[dst])


if __name__ == "__main__":
    main()
```
