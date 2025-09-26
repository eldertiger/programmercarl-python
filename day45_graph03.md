# 图论 part 3

## [101. 孤岛的总面积](https://www.programmercarl.com/kamacoder/0101.%E5%AD%A4%E5%B2%9B%E7%9A%84%E6%80%BB%E9%9D%A2%E7%A7%AF.html)

### DFS
将所有和边界相邻的岛屿全部置为0，统计grid剩下的和即使孤岛的总面积。  
即从四个边进行遍历，将找到的岛屿全部置0。
```python
N,M = map(int, input().split())
grid = [[0]*M for _ in range(N)]
for i in range(N):
    grid[i] = list(map(int,input().split()))

directions = [[1,0],[0,1],[-1,0],[0,-1]]
def dfs(grid,visited,x,y):
    if visited[x][y] or not grid[x][y]:
        return
    visited[x][y] = True
    grid[x][y] = 0
    for i,j in directions:
        next_x = x + i
        next_y = y + j
        if next_x < 0 or next_x >= len(grid) or next_y < 0 or next_y >= len(grid[0]):
            continue
        if grid[next_x][next_y] == 1: 
            dfs(grid, visited, next_x, next_y)
       
visited = [[False]*M for _ in range(N)]

for i in range(N):
    if grid[i][0] == 1:
        dfs(grid,visited,i,0)
    if grid[i][M-1] == 1:
        dfs(grid,visited,i,M-1)

for j in range(M):
    if grid[0][j] == 1:
        dfs(grid,visited,0,j)
    if grid[N-1][j] == 1:
        dfs(grid,visited,N-1,j)
area = 0
for i in range(len(grid)):
    for j in range(len(grid[0])):
        if grid[i][j] == 1:
            area += 1
print(area)
```

### BFS

```python
from collections import deque
N,M = map(int, input().split())
grid = [[0]*M for _ in range(N)]
for i in range(N):
    grid[i] = list(map(int,input().split()))

directions = [[1,0],[0,1],[-1,0],[0,-1]]
def bfs(grid,visited,x,y):
    queue = deque()
    queue.append([x,y])
    visited[x][y] = True
    grid[x][y] = 0
    while queue:
        x,y = queue.popleft()
        for i,j in directions:
            next_x = x + i
            next_y = y + j
            if next_x < 0 or next_x >= len(grid) or next_y < 0 or next_y >= len(grid[0]):
                continue
            if grid[next_x][next_y] == 1 and not visited[next_x][next_y]: 
                bfs(grid, visited, next_x, next_y)
       
visited = [[False]*M for _ in range(N)]

for i in range(N):
    if grid[i][0] == 1:
        bfs(grid,visited,i,0)
    if grid[i][M-1] == 1:
        bfs(grid,visited,i,M-1)

for j in range(M):
    if grid[0][j] == 1:
        bfs(grid,visited,0,j)
    if grid[N-1][j] == 1:
        bfs(grid,visited,N-1,j)
area = 0
for i in range(len(grid)):
    for j in range(len(grid[0])):
        if grid[i][j] == 1:
            area += 1
print(area)
```

## [102. 沉没孤岛](https://www.programmercarl.com/kamacoder/0102.%E6%B2%89%E6%B2%A1%E5%AD%A4%E5%B2%9B.html)

### dfs
优化上一题的visited，直接将靠近边界的岛屿值赋值成2，然后最后再将孤岛的1变为0，其他的2变为1。
```python
N,M = map(int, input().split())
grid = [[0]*M for _ in range(N)]
for i in range(N):
    grid[i] = list(map(int,input().split()))

directions = [[1,0],[0,1],[-1,0],[0,-1]]
def dfs(grid,x,y):
    if  grid[x][y] == 0 or grid[x][y]==2:
        return
    grid[x][y] = 2
    for i,j in directions:
        next_x = x + i
        next_y = y + j
        if next_x < 0 or next_x >= len(grid) or next_y < 0 or next_y >= len(grid[0]):
            continue
        if grid[next_x][next_y] == 1: 
            dfs(grid, next_x, next_y)
       

for i in range(N):
    if grid[i][0] == 1:
        dfs(grid,i,0)
    if grid[i][M-1] == 1:
        dfs(grid,i,M-1)

for j in range(M):
    if grid[0][j] == 1:
        dfs(grid,0,j)
    if grid[N-1][j] == 1:
        dfs(grid,N-1,j)
for i in range(len(grid)):
    for j in range(len(grid[0])):
        if grid[i][j] == 1:
            grid[i][j]=0
        if grid[i][j]==2:
            grid[i][j]=1
for i in range(N):
    print(" ".join(map(str,grid[i])))
```
### bfs
```python
from collections import deque
N,M = map(int, input().split())
grid = [[0]*M for _ in range(N)]
for i in range(N):
    grid[i] = list(map(int,input().split()))

directions = [[1,0],[0,1],[-1,0],[0,-1]]
def bfs(grid,x,y):
    queue = deque()
    queue.append([x,y])
    grid[x][y] = 2
    while queue:
        x,y = queue.popleft()
        for i,j in directions:
            next_x = x + i
            next_y = y + j
            if next_x < 0 or next_x >= len(grid) or next_y < 0 or next_y >= len(grid[0]):
                continue
            if grid[next_x][next_y] == 1: 
                bfs(grid, next_x, next_y)
       

for i in range(N):
    if grid[i][0] == 1:
        bfs(grid,i,0)
    if grid[i][M-1] == 1:
        bfs(grid,i,M-1)

for j in range(M):
    if grid[0][j] == 1:
        bfs(grid,0,j)
    if grid[N-1][j] == 1:
        bfs(grid,N-1,j)

for i in range(len(grid)):
    for j in range(len(grid[0])):
        if grid[i][j] == 1:
            grid[i][j]=0
        if grid[i][j]==2:
            grid[i][j]=1
for i in range(N):
    print(" ".join(map(str,grid[i])))
```


## [103. 高山流水](https://www.programmercarl.com/kamacoder/0103.%E6%B0%B4%E6%B5%81%E9%97%AE%E9%A2%98.html)

### dfs
逆向思维，从边界出发寻找可以流到边界的点，两个边界同时都能流到的就是目标的分水岭。
```python
N,M = map(int, input().split())
grid = [[0]*M for _ in range(N)]
for i in range(N):
    grid[i] = list(map(int,input().split()))

directions = [[1,0],[0,1],[-1,0],[0,-1]]

def dfs(grid,visited,x,y):
    if visited[x][y]:
        return
    visited[x][y] = True
    for i,j in directions:
        next_x = x + i
        next_y = y + j
        if next_x < 0 or next_x >= len(grid) or next_y < 0 or next_y >= len(grid[0]):
            continue
        if grid[next_x][next_y] >= grid[x][y]: 
            dfs(grid, visited, next_x, next_y)

leftupper_bound = [[False]*M for _ in range(N)]
rightbottom_bound = [[False]*M for _ in range(N)]
for i in range(N):
    dfs(grid,leftupper_bound,i,0)
    dfs(grid,rightbottom_bound,i,M-1)
for j in range(M):
    dfs(grid,leftupper_bound,0,j)
    dfs(grid,rightbottom_bound,N-1,j)
res = []
for i in range(N):
    for j in range(M):
        if leftupper_bound[i][j] and rightbottom_bound[i][j]:
            res.append([i,j])
for i in range(len(res)):
    print(" ".join(map(str,res[i])))
```
## [104.建造最大岛屿](https://www.programmercarl.com/kamacoder/0104.%E5%BB%BA%E9%80%A0%E6%9C%80%E5%A4%A7%E5%B2%9B%E5%B1%BF.html)
计算与每个海洋格点相邻的所有岛屿的面积

### dfs

```python
N,M = map(int, input().split())
grid = [[0]*M for _ in range(N)]
for i in range(N):
    grid[i] = list(map(int,input().split()))

directions = [[1,0],[0,1],[-1,0],[0,-1]]

def dfs(grid,visited,x,y):
    if visited[x][y]:
        return
    visited[x][y] = True
    global area
    area += 1
    for i,j in directions:
        next_x = x + i
        next_y = y + j
        if next_x < 0 or next_x >= len(grid) or next_y < 0 or next_y >= len(grid[0]):
            continue
        if grid[next_x][next_y] == 1 and not visited[next_x][next_y]: 
            dfs(grid, visited, next_x, next_y)
max_area = 0
for i in range(N):
    for j in range(M):
        max_area += grid[i][j]
if max_area == N*M:
    print(max_area)
else:
    max_area = 0         
    for i in range(N):
        for j in range(M):
            if grid[i][j] == 0:
                visited  = [[False]*M for _ in range(N)]
                area = 0
                dfs(grid,visited,i,j)
                max_area = max(max_area,area)

    print(max_area)

```