# 图论 part2

## [99. 岛屿数量 深搜](https://www.programmercarl.com/kamacoder/0099.%E5%B2%9B%E5%B1%BF%E7%9A%84%E6%95%B0%E9%87%8F%E6%B7%B1%E6%90%9C.html)

### DFS 1
遇到陆地时，岛屿数量+1，赋值为True，然后用dfs将所有相邻陆地的visited赋值为True, 直到最后遍历完所有的格点。


```python
N,M = map(int, input().split())
graph = [[0]*M for _ in range(N)]
for i in range(N):
    graph[i][:] = list(map(int,input().split()))

direction = [[1,0],[0,1],[-1,0],[0,-1]]

def dfs(graph,x,y,visited):
    for i, j in direction:
        nextx = x + i    
        nexty = y + j
        if nextx<0 or nextx>=len(graph) or nexty < 0 or nexty >= len(graph[0]):
            continue
        if not visited[nextx][nexty] and graph[nextx][nexty]==1:
            visited[nextx][nexty]  = True
            dfs(graph,nextx,nexty,visited)

res = 0
visited = [[False]*M for _ in range(N)]
for i in range(N):
    for j in range(M):
        if not visited[i][j] and graph[i][j]==1:
            res += 1
            visited[i][j] = True
            dfs(graph,i,j,visited)
print(res)
```

### DFS 2
遇到陆地，数量+1，然后dfs将该地和所有相邻陆地的visited全部置为True；
注意此处DFS先判断返回条件，海洋或已经访问过了；所以后面直接dfs，而不用先判断是否合法。
理论上第一种先判断是否合法的方法更好些。

```python
N,M = map(int, input().split())
graph = [[0]*M for _ in range(N)]
for i in range(N):
    graph[i][:] = list(map(int,input().split()))

direction = [[1,0],[0,1],[-1,0],[0,-1]]

def dfs(graph,x,y,visited):
    if visited[x][y] or graph[x][y] == 0:
        return
    visited[x][y] = True
    for i, j in direction:
        nextx = x + i    
        nexty = y + j
        if nextx<0 or nextx>=len(graph) or nexty < 0 or nexty >= len(graph[0]):
            continue
        dfs(graph,nextx,nexty,visited)

res = 0
visited = [[False]*M for _ in range(N)]
for i in range(N):
    for j in range(M):
        if not visited[i][j] and graph[i][j]==1:
            res += 1
            dfs(graph,i,j,visited)
print(res)
```

## [99. 岛屿数量 广搜](https://www.programmercarl.com/kamacoder/0099.%E5%B2%9B%E5%B1%BF%E7%9A%84%E6%95%B0%E9%87%8F%E5%B9%BF%E6%90%9C.html)

### BFS
遍历所有格点，遇到陆地，BFS遍历所有相邻陆地；
BFS每个格点加入队列，加入时即标记访问过了，然后向四个方向判断。
```python
from collections import deque
N,M = map(int, input().split())
graph = [[0]*M for _ in range(N)]
for i in range(N):
    graph[i][:] = list(map(int,input().split()))

direction = [[1,0],[0,1],[-1,0],[0,-1]]

def bfs(graph,x,y,visited):
    queue = deque()
    queue.append([x,y])
    visited[x][y] = True
    while queue:
        curx,cury = queue.popleft()
        for i, j in direction:
            nextx = curx + i
            nexty = cury + j
            if nextx<0 or nextx>=len(graph) or nexty < 0 or nexty >= len(graph[0]):
                continue
            if not visited[nextx][nexty] and graph[nextx][nexty]:
                bfs(graph,nextx,nexty,visited)


res = 0
visited = [[False]*M for _ in range(N)]
for i in range(N):
    for j in range(M):
        if not visited[i][j] and graph[i][j]==1:
            res += 1
            bfs(graph,i,j,visited)
print(res)
```
## [100. 岛屿的最大面积](https://www.programmercarl.com/kamacoder/0100.%E5%B2%9B%E5%B1%BF%E7%9A%84%E6%9C%80%E5%A4%A7%E9%9D%A2%E7%A7%AF.html)
在前面深搜和广搜的基础上添加一个面积统计。

### DFS
```python
direction = [[0,1],[1,0],[-1,0],[0,-1]]
def dfs(graph,visited,x,y):
    if visited[x][y] or not graph[x][y]:
        return
    visited[x][y] = True
    global area
    area += 1
    for i, j in direction:
        nextx = x + i
        nexty = y + j
        if nextx < 0 or nextx >= len(graph) or nexty < 0 or nexty >= len(graph[0]):
            continue
        dfs(graph,visited,nextx,nexty)


N,M = map(int,input().split())
graph = [[0]*M for _ in range(N)]
for i in range(N):
    graph[i][:] = list(map(int,input().split()))
max_area = 0
visited = [[False]*M for _ in range(N)]
for i in range(N):
    for j in range(M):
        if not visited[i][j] and graph[i][j]:
            area = 0
            dfs(graph,visited,i,j,area)
            max_area = max(max_area,area)
print(max_area)

```

### BFS 

```python
from collections import deque
N,M = map(int, input().split())
graph = [[0]*M for _ in range(N)]
for i in range(N):
    graph[i][:] = list(map(int,input().split()))

direction = [[1,0],[0,1],[-1,0],[0,-1]]

def bfs(graph,x,y,visited):
    queue = deque()
    queue.append([x,y])
    visited[x][y] = True
    global area
    area+=1
    while queue:
        curx,cury = queue.popleft()
        for i, j in direction:
            nextx = curx + i
            nexty = cury + j
            if nextx<0 or nextx>=len(graph) or nexty < 0 or nexty >= len(graph[0]):
                continue
            if not visited[nextx][nexty] and graph[nextx][nexty]:
                bfs(graph,nextx,nexty,visited)


max_area = 0
visited = [[False]*M for _ in range(N)]

for i in range(N):
    for j in range(M):
        if not visited[i][j] and graph[i][j]==1:
            area = 0
            bfs(graph,i,j,visited)
            max_area = max(max_area,area)
print(max_area)
```