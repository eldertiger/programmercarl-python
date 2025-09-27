# 图论 part4

## [110. 字符串接龙](https://www.programmercarl.com/kamacoder/0110.%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%8E%A5%E9%BE%99.html)
### 思路 BFS
首先需要连接所有只差一个字符的字符串；  
然后寻找最短路径；  
最短路径使用BFS来寻找，第一个找到终点的路径就是最短路径长度；  

```python
from collections import deque
n = int(input())

begin_str, end_str = map(str,input().split())
if begin_str == end_str:
    print(0)
    exit()
s = []
for _ in range(n):
    s.append(input())

def judge(s1,s2):
    if len(s1) == len(s2):
        count = 0
        for i in range(len(s1)):
            if s1[i] != s2[i]:
                count += 1
        return count == 1
    else:
        return False
visited = [False]*n
queue = deque()
queue.append([begin_str,1])
while queue:
    cur_str, step = queue.popleft()
    if judge(cur_str,end_str):
        print(step+1)
        exit()
    for i in range(n):
        if not visited[i] and judge(s[i],cur_str):
            visited[i] = True
            queue.append([s[i],step+1])

print(0)
```

## [105.有向图的完全联通](https://www.programmercarl.com/kamacoder/0105.%E6%9C%89%E5%90%91%E5%9B%BE%E7%9A%84%E5%AE%8C%E5%85%A8%E5%8F%AF%E8%BE%BE%E6%80%A7.html#%E6%80%9D%E8%B7%AF)
### DFS
使用邻接表存储；  
从1号节点开始深度优先搜索，访问过的节点全部标记为True，若最后有节点没有被访问，则输出-1。  
打印之后需要exit()！！！
```python
from collections import defaultdict
n,k = map(int, input().split())
graph = defaultdict(list)
for i in range(k):
    x,y = map(int,input().split())
    graph[x].append(y)

def dfs(graph,visited,x):
    if visited[x]:
        return
    visited[x] = True
    for y in graph[x]:
        dfs(graph,visited,y)
visited = [False]*(n+1)
dfs(graph,visited,1)    
for i in range(1,n+1):
    if not visited[i]:
        print(-1)
        exit()
print(1)
```

## [106. 岛屿的周长](https://www.programmercarl.com/kamacoder/0106.%E5%B2%9B%E5%B1%BF%E7%9A%84%E5%91%A8%E9%95%BF.html)

### 思路
统计所有岛屿格点周围的海洋格点或者边界的数量(可重复计数)就是岛屿的周长；  
若是反过来统计海洋格点或者边界格点周围的陆地格点，也是可以的，但是写起来比较麻烦。

```python
n,m = map(int,input().split())
grid = [[0]*m for _ in range(n)]
for i in range(n):
    grid[i] = list(map(int,input().split()))

directions = [[1,0],[0,1],[-1,0],[0,-1]]
perimeter = 0
for i in range(n):
    for j in range(m):
        if grid[i][j] == 1:
            for x,y in directions:
                next_x = i + x
                next_y = j + y
                if next_x<0 or next_x>=n or next_y < 0 or next_y >= m or grid[next_x][next_y]==0:
                    perimeter += 1
print(perimeter)
```