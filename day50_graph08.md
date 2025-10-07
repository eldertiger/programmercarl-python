# 图论 part8

## [拓扑排序精讲](https://www.programmercarl.com/kamacoder/0117.%E8%BD%AF%E4%BB%B6%E6%9E%84%E5%BB%BA.html)

### 卡码网：117. 软件构建(opens new window)

### 题目描述：

某个大型软件项目的构建系统拥有 N 个文件，文件编号从 0 到 N - 1，在这些文件中，某些文件依赖于其他文件的内容，这意味着如果文件 A 依赖于文件 B，则必须在处理文件 A 之前处理文件 B （0 <= A, B <= N - 1）。请编写一个算法，用于确定文件处理的顺序。

### 输入描述：

第一行输入两个正整数 N, M。表示 N 个文件之间拥有 M 条依赖关系。

后续 M 行，每行两个正整数 S 和 T，表示 T 文件依赖于 S 文件。

### 输出描述：

输出共一行，如果能处理成功，则输出文件顺序，用空格隔开。

如果不能成功处理（相互依赖），则输出 -1。

### 输入示例：
```
5 4
0 1
0 2
1 3
2 4
```
### 输出示例：

0 1 2 3 4

### 思路
给出一个 有向图，把这个有向图转成线性的排序 就叫拓扑排序。

当然拓扑排序也要检测这个有向图 是否有环，即存在循环依赖的情况，因为这种情况是不能做线性排序的。

所以拓扑排序也是图论中判断有向无环图的常用方法。

可以采用BFS或者DFS；以下采用BFS。
1. 寻找入度为0的节点，可以有多个，加入队列；
2. 对于每个入度为0的节点，加入result，将其从图中删除，即将依赖于该节点的节点入度减一，若后者入度变为0，则加入队列；
3. 若最终结果与节点等长，则输出打印队列，若不等长，说明图中存在环，输出-1

```python
from collections import deque, defaultdict
n,m = map(int, input().split())
edges = [tuple(map(int, input().split()))for _ in range(m)]

def bfs(n,edges):
    indegrees = [0]*n
    umap = defaultdict(list)
    for s,t in edges:
        indegrees[t] += 1
        umap[s].append(t)
        
    queue = deque([i for i in range(n) if indegrees[i] == 0])
    result = []
    while queue:
        s  = queue.popleft()
        result.append(s)
        for i in umap[s]:
            indegrees[i] -= 1
            if indegrees[i] == 0:
                queue.append(i)


    if len(result) == n:
        print(" ".join(map(str,result)))
    else:
        print(-1)
bfs(n, edges)
```

## [dijkstra（朴素版）精讲](https://www.programmercarl.com/kamacoder/0047.%E5%8F%82%E4%BC%9Adijkstra%E6%9C%B4%E7%B4%A0.html#%E6%80%9D%E8%B7%AF)

### 卡码网：47. 参加科学大会(opens new window)

### 【题目描述】

小明是一位科学家，他需要参加一场重要的国际科学大会，以展示自己的最新研究成果。

小明的起点是第一个车站，终点是最后一个车站。然而，途中的各个车站之间的道路状况、交通拥堵程度以及可能的自然因素（如天气变化）等不同，这些因素都会影响每条路径的通行时间。

小明希望能选择一条花费时间最少的路线，以确保他能够尽快到达目的地。

### 【输入描述】

第一行包含两个正整数，第一个正整数 N 表示一共有 N 个公共汽车站，第二个正整数 M 表示有 M 条公路。

接下来为 M 行，每行包括三个整数，S、E 和 V，代表了从 S 车站可以单向直达 E 车站，并且需要花费 V 单位的时间。

### 【输出描述】

输出一个整数，代表小明从起点到终点所花费的最小时间。

### 输入示例
```
7 9
1 2 1
1 3 4
2 3 2
2 4 5
3 4 2
4 5 3
2 6 4
5 7 4
6 7 9
```
### 输出示例：
12


### 思路

类似prim算法，但是minDist更新的是从当前节点到源节点的最近距离权重，而prim是当前节点到生成树的最近距离，前者需要加上对应生成树的最近节点到源节点的最短距离。

```python
import sys

def dijkstra(n, m, grid, start, end):


    # 初始化距离数组和访问数组
    minDist = [float('inf')] * (n + 1)
    visited = [False] * (n + 1)

    minDist[start] = 0  # 起始点到自身的距离为0

    for _ in range(1, n + 1):  # 遍历所有节点
        minVal = float('inf')
        cur = -1

        # 选择距离源点最近且未访问过的节点
        for v in range(1, n + 1):
            if not visited[v] and minDist[v] < minVal:
                minVal = minDist[v]
                cur = v

        if cur == -1:  # 如果找不到未访问过的节点，提前结束
            break

        visited[cur] = True  # 标记该节点已被访问

        # 更新未访问节点到源点的距离
        for v in range(1, n + 1):
            if not visited[v] and grid[cur][v] != float('inf') and minDist[cur] + grid[cur][v] < minDist[v]:
                minDist[v] = minDist[cur] + grid[cur][v]

    if minDist[end] == float('inf'):
        return -1 
    else:
        return minDist[end]

n, m = map(int,input().split())
grid = [[float('inf')]*(n+1)for _ in range(n+1)]
for _ in range(m):
    s, e, v = map(int,input().split())
    grid[s][e] = v
start = 1  # 起点
end = n    # 终点
result = dijkstra(n, m, grid, start, end)
print(result)
```