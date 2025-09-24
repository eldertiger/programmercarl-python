# 图论 part1
## [98. 所有可达路径](https://www.programmercarl.com/kamacoder/0098.%E6%89%80%E6%9C%89%E5%8F%AF%E8%BE%BE%E8%B7%AF%E5%BE%84.html)

ACM题目，主要考察邻接表和邻接矩阵的写法
### 邻接表

```python
from collections import defaultdict
N, M = input().split(" ")
M, N = int(M), int(N)
graph = defaultdict(list)
for i in range(M):
    s,t = map(int, input().split(" "))
    graph[s].append(t)

def dfs(graph,x,n,path,res):
    if x == n:
        res.append(path.copy())
        return
    for i in graph[x]:
        path.append(i)
        dfs(graph,i,n,path,res)
        path.pop()
res = []
dfs(graph,1,N,[1],res)
if res:
    for path in res:
        print(" ".join(map(str,path)))
else:
    print(-1)

```

### 邻接矩阵
```python
N, M = input().split(" ")
M, N = int(M), int(N)
graph = [[0]*(N+1) for _ in range(N+1)]
for i in range(M):
    s,t = map(int, input().split(" "))
    graph[s][t]=1

def dfs(graph,x,n,path,res):
    if x == n:
        res.append(path.copy())
        return
    for i in range(1,n+1):
        if graph[x][i]==1:
            path.append(i)
            dfs(graph,i,n,path,res)
            path.pop()
res = []
dfs(graph,1,N,[1],res)
if res:
    for path in res:
        print(" ".join(map(str,path)))
else:
    print(-1)

```



## [797. 所有可能的路径](https://leetcode.cn/problems/all-paths-from-source-to-target/description/)


```python
class Solution:
    def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:
        def dfs(graph,x,n,path,res):
            if x == n-1:
                res.append(path.copy())
                return
            for i in graph[x]:
                path.append(i)
                dfs(graph,i,n,path,res)
                path.pop()
        res = []
        n = len(graph)
        dfs(graph,0,n,[0],res)
        return res
```