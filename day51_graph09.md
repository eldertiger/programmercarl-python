# 图论 part9

## [dijkstra（堆优化版）精讲](https://www.programmercarl.com/kamacoder/0047.%E5%8F%82%E4%BC%9Adijkstra%E5%A0%86.html)

### 思路

使用堆序替换寻找最小的权重距离，对dijkstra算法(类似kruskal版本)进行优化，从O(n^2)优化到O(nlogn)。

```python
import heapq

class Edge:
    def __init__(self, to, val):
        self.to = to
        self.val = val
def dijkstra(n, m, grid, start, end):
    grid = [[] for _ in range(n+1)]

    for v1, v2, val in edges:
        grid[v1].append(Edge(v2, val))
    minDist = [float('inf')]*(n+1)
    visited = [False] * (n+1)    
    pq = []
    heapq.heappush(pq, (0, start))
    minDist[start] = 0  # 起始点到自身的距离为0
    
    while pq:
        cur_dist, cur_node = heapq.heappop(pq)
        if visited[cur_node]:
            continue
        visited[cur_node] = True
        for edge in grid[cur_node]:
            if not visited[edge.to] and cur_dist+edge.val < minDist[edge.to]:
                minDist[edge.to] = cur_dist + edge.val
                heapq.heappush(pq, (minDist[edge.to],edge.to))

    if minDist[end] == float('inf'):
        return -1 
    else:
        return minDist[end]

n, m = map(int,input().split())
edges = [tuple(map(int, input().split())) for  _ in range(m)]
start = 1  # 起点
end = n    # 终点
result = dijkstra(n, m, edges, start, end)
print(result)

```

## [Bellman_ford 算法精讲](https://www.programmercarl.com/kamacoder/0094.%E5%9F%8E%E5%B8%82%E9%97%B4%E8%B4%A7%E7%89%A9%E8%BF%90%E8%BE%93I.html#%E6%80%9D%E8%B7%AF)

### 题目描述

某国为促进城市间经济交流，决定对货物运输提供补贴。共有 n 个编号为 1 到 n 的城市，通过道路网络连接，网络中的道路仅允许从某个城市单向通行到另一个城市，不能反向通行。

网络中的道路都有各自的运输成本和政府补贴，道路的权值计算方式为：运输成本 - 政府补贴。

权值为正表示扣除了政府补贴后运输货物仍需支付的费用；权值为负则表示政府的补贴超过了支出的运输成本，实际表现为运输过程中还能赚取一定的收益。

请找出从城市 1 到城市 n 的所有可能路径中，综合政府补贴后的最低运输成本。

如果最低运输成本是一个负数，它表示在遵循最优路径的情况下，运输过程中反而能够实现盈利。

城市 1 到城市 n 之间可能会出现没有路径的情况，同时保证道路网络中不存在任何负权回路。

负权回路是指一系列道路的总权值为负，这样的回路使得通过反复经过回路中的道路，理论上可以无限地减少总成本或无限地增加总收益。

### 输入描述

第一行包含两个正整数，第一个正整数 n 表示该国一共有 n 个城市，第二个整数 m 表示这些城市中共有 m 条道路。

接下来为 m 行，每行包括三个整数，s、t 和 v，表示 s 号城市运输货物到达 t 号城市，道路权值为 v（单向图）。

### 输出描述

如果能够从城市 1 到连通到城市 n， 请输出一个整数，表示运输成本。如果该整数是负数，则表示实现了盈利。如果从城市 1 没有路径可达城市 n，请输出 "unconnected"。

### 输入示例：
```
6 7
5 6 -2
1 2 1
5 3 1
2 5 2
2 4 -3
4 6 4
1 3 5
```
### 思路

dijkstra只能用于非负数权重图；
存在负数的权重图需要使用Bellman_ford算法；
核心思想是 对所有边进行松弛n-1次操作（n为节点数量），从而求得目标最短路。

一条边，节点A 到 节点B 权值为value，minDist[B] 表示 到达B节点 最小权值，minDist[B] 有哪些状态可以推出来？

状态一： minDist[A] + value 可以推出 minDist[B] 
状态二： minDist[B]本身就有权值 （可能是其他边链接的节点B 例如节点C，以至于 minDist[B]记录了其他边到minDist[B]的权值）
minDist[B] 要求最小权值，那么 这两个状态我们就取最小的
```
if (minDist[B] > minDist[A] + value) minDist[B] = minDist[A] + value
```
这个过程就叫做 “松弛” 。所谓松弛就是通过比较miniDist[B]的原有权值和通过某个路径到B的新权值来更新一次minDist[B].

```python
n, m = map(int, input().split())
edges = []
for _ in range(m):
    src, dest, weight = map(int, input().split())
    edges.append([src,dest, weight])

minDist = [float('inf')]*(n+1)
minDist[1] = 0# 起点距离为0

for i in range(1,n):
    updated = False
    for src, dest, weight in edges:
        if minDist[src] != float("inf") and minDist[src] + weight < minDist[dest]:
            minDist[dest] = minDist[src] + weight
            updated = True
    if not updated:
            break
if minDist[-1] == float("inf"):
    print("unconnected")
else:
    print(minDist[-1])

```