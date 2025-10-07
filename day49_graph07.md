# 图论 part7

## [prim算法精讲](https://www.programmercarl.com/kamacoder/0053.%E5%AF%BB%E5%AE%9D-prim.html)
### 题目描述：

在世界的某个区域，有一些分散的神秘岛屿，每个岛屿上都有一种珍稀的资源或者宝藏。国王打算在这些岛屿上建公路，方便运输。

不同岛屿之间，路途距离不同，国王希望你可以规划建公路的方案，如何可以以最短的总公路距离将所有岛屿联通起来。

给定一张地图，其中包括了所有的岛屿，以及它们之间的距离。以最小化公路建设长度，确保可以链接到所有岛屿。

### 输入描述：

第一行包含两个整数V和E，V代表顶点数，E代表边数。顶点编号是从1到V。例如：V=2，一个有两个顶点，分别是1和2。

接下来共有E行，每行三个整数v1，v2和val，v1和v2为边的起点和终点，val代表边的权值。

### 输出描述：

输出联通所有岛屿的最小路径总距离

### 输入示例：
```
7 11
1 2 1
1 3 1
1 5 2
2 6 1
2 4 2
2 3 2
3 4 1
4 5 1
5 6 2
5 7 1
6 7 1
```
### 输出示例：

6

### 思路
n个点，M条边，找出N-1条边，保证将所有节点连接的总长度最短；  
使用一个最小距离矩阵minDist，存储每个节点到已经生成的树最短的距离以及对应的边(两个节点);  
遍历所有节点，从每个节点出发，选取离当前节点最近的节点作为下一次遍历的起点，然后更新所有不在树中的节点的minDist为当前minDist值和与当前节点距离的较小值，直到遍历完所有的节点，那么minDist的距离之和就是最短距离；  

```python
from collections import defaultdict
v, e = map(int, input().split())
graph = [[10001]*(v+1) for _ in range(v+1)]
for i in range(e):
    v1, v2, val = map(int, input().split())
    graph[v1][v2]=val
    graph[v2][v1]=val
visited = [False]*(v+1)
minDist = [10001]*(v+1)

for _ in range(1,v+1):
    minVal = 10002
    cur = -1
    for i in range(1, v+1):
        if visited[i] == False and minDist[i] < minVal:
            cur = i
            minVal = minDist[i]
    visited[cur] = True
    for i in range(1, v+1):
        if visited[i] == False and graph[cur][i] < minDist[i]:
            minDist[i] = graph[cur][i]
print(sum(minDist[2:v+1]))
    
```

## [kruskal算法精讲](https://www.programmercarl.com/kamacoder/0053.%E5%AF%BB%E5%AE%9D-Kruskal.html#%E8%A7%A3%E9%A2%98%E6%80%9D%E8%B7%AF)

题目同上；

### 思路
从贪心角度来说，一定是优先获取所有较短的边相连接的树的权值和最短；
首先对所有边的权值排序，选最短的一条，然后从剩下的边中继续从小到大选取，若新选取边的两个节点在同一个集合中（需要用到并查集），即构成了环，则跳过，直到所有节点都在树中（剩余的边都会构成环），则当前权值和最小。

```python

class Edge:
    def __init__(self, l, r, val):
        self.l = l
        self.r = r
        self.val = val

n = 10001
father = list(range(n))
def init():
    global father
    father = list(range(n))
def find(u):
    if u != father[u]:
        father[u] = find(father[u])
    return father[u]
def join(u,v):
    root_u = find(u) 
    root_v = find(v)
    if root_u != root_v: father[v] = u
def kruskal(v, edges):
    edges.sort(key=lambda edge:edge.val)
    init()
    result_val = 0
    for edge in edges:
        x = find(edge.l)
        y = find(edge.r)
        if x!=y:
            result_val += edge.val
            join(x,y)
    print(result_val)
v, e = map(int, input().split())

edges = []
for i in range(e):
    v1, v2, val = map(int, input().split())
    edges.append(Edge(v1, v2, val))

kruskal(v,edges)
```


