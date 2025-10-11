# 图论 part11

## [Floyd 算法精讲](https://www.programmercarl.com/kamacoder/0097.%E5%B0%8F%E6%98%8E%E9%80%9B%E5%85%AC%E5%9B%AD.html)

### 【题目描述】

小明喜欢去公园散步，公园内布置了许多的景点，相互之间通过小路连接，小明希望在观看景点的同时，能够节省体力，走最短的路径。

给定一个公园景点图，图中有 N 个景点（编号为 1 到 N），以及 M 条双向道路连接着这些景点。每条道路上行走的距离都是已知的。

小明有 Q 个观景计划，每个计划都有一个起点 start 和一个终点 end，表示他想从景点 start 前往景点 end。由于小明希望节省体力，他想知道每个观景计划中从起点到终点的最短路径长度。 请你帮助小明计算出每个观景计划的最短路径长度。

### 【输入描述】

第一行包含两个整数 N, M, 分别表示景点的数量和道路的数量。

接下来的 M 行，每行包含三个整数 u, v, w，表示景点 u 和景点 v 之间有一条长度为 w 的双向道路。

接下里的一行包含一个整数 Q，表示观景计划的数量。

接下来的 Q 行，每行包含两个整数 start, end，表示一个观景计划的起点和终点。

### 【输出描述】

对于每个观景计划，输出一行表示从起点到终点的最短路径长度。如果两个景点之间不存在路径，则输出 -1。

### 【输入示例】
```
7 3
2 3 4
3 6 6
4 7 8
2
2 3
3 4
```
### 【输出示例】
```
4 -1
```
### 【提示信息】

从 1 到 2 的路径长度为 4，2 到 3 之间并没有道路。

1 <= N, M, Q <= 1000.

### 思路 多源最短路径问题
使用递归来解决，i到j节点可以从i到k,k再到j节点的最小距离之和得到，其中k为1-n节点。   

$grid[i][j][k] = min(grid[i][k][k - 1] + grid[k][j][k - 1]， grid[i][j][k - 1])$

```python
n,m = map(int, input().split())
grid = [[[10001]*(n+1)for _ in range(n+1)]for _ in range(n+1)]

for _ in range(m):
    u,v,w = map(int,input().split())
    grid[u][v][0] = w
    grid[v][u][0] = w

for k in range(1, n+1):
    for i in range(1, n+1):
        for j in range(1, n+1):
            grid[i][j][k] = min(grid[i][j][k-1], grid[i][k][k-1]+grid[k][j][k-1])
q = int(input())
for _ in range(q):
    start, end = map(int, input().split())
    if grid[start][end][n] == 10001:
        print(-1)
    else:
        print(grid[start][end][n])
```

### 优化为二维数组

```python
n,m = map(int, input().split())
grid = [[10001]*(n+1)for _ in range(n+1)]

for _ in range(m):
    u,v,w = map(int,input().split())
    grid[u][v] = w
    grid[v][u] = w

for k in range(1, n+1):
    for i in range(1, n+1):
        for j in range(1, n+1):
            grid[i][j] = min(grid[i][j], grid[i][k]+grid[k][j])
q = int(input())
for _ in range(q):
    start, end = map(int, input().split())
    if grid[start][end] == 10001:
        print(-1)
    else:
        print(grid[start][end])
```
## [A*算法精讲](https://www.programmercarl.com/kamacoder/0126.%E9%AA%91%E5%A3%AB%E7%9A%84%E6%94%BB%E5%87%BBastar.html)

### 卡码网：126. 骑士的攻击(opens new window)

### 题目描述

在象棋中，马和象的移动规则分别是“马走日”和“象走田”。现给定骑士的起始坐标和目标坐标，要求根据骑士的移动规则，计算从起点到达目标点所需的最短步数。

骑士移动规则如图，红色是起始位置，黄色是骑士可以走的地方。



棋盘大小 1000 x 1000（棋盘的 x 和 y 坐标均在 [1, 1000] 区间内，包含边界）

### 输入描述

第一行包含一个整数 n，表示测试用例的数量。

接下来的 n 行，每行包含四个整数 a1, a2, b1, b2，分别表示骑士的起始位置 (a1, a2) 和目标位置 (b1, b2)。

### 输出描述

输出共 n 行，每行输出一个整数，表示骑士从起点到目标点的最短路径长度。

### 输入示例
```
6
5 2 5 4
1 1 2 2
1 1 8 8
1 1 8 7
2 1 3 3
4 6 4 6
```
### 输出示例
```
2
4
6
5
1
0
```

### 思路

贪心算法，马可以走8个方向，A*算法最终会朝着目标欧拉距离最近的点位移动，而非漫无目的的查找，本题中通过更新到达每一个点位的步数来实现这一点。


```python
import heapq
n = int(input())

moves = [(1,2),(2,1),(-1,2),(2,-1),(-2,1),(1,-2),(-1,-2),(-2,-1)]

def distance(a,b):
    return ((a[0]-b[0])**2+(a[1]-b[1])**2)**0.5

def bfs(start,end):
    q = [distance(start, end), start]
    step = {start: 0}
    while q:
        d, cur = heapq.heappop(q)
        if cur == end:
            return step[cur]
        for move in moves:
            new = (move[0] + cur[0], move[1]+ cur[1])
            if 1 <= new[0] <=1000 and 1 <=new[1]<=1000:
                step_new = step[cur] + 1
                if step_new < step.get(new, float('inf')):
                    step[new] = step_new
                    heapq.heappush(q, (distance(new, end)+step_new, new))
    return False

for _  in range(n):
    a1, a2, b1, b2 = map(int, input().split())
    print(bfs((a1, a2),(b1, b2)))

```
