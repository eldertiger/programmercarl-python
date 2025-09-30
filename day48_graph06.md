# 图论 part5

## [108. 冗余连接](https://www.programmercarl.com/kamacoder/0108.%E5%86%97%E4%BD%99%E8%BF%9E%E6%8E%A5.html)

### 并查集
并查集的简单应用，由于题设说明了在冗余边出现之前，原来是一个树形结构，所以只需要判断冗余边出现时对应的两个点是否已经拥有同一个根节点就可以了，也就是在同一个并查集中。

```python
class UnionFind:
    def __init__(self,size):
        self.parent = list(range(size+1))
    def find(self,u):
        if u != self.parent[u]:
            self.parent[u] = self.find(self.parent[u])
        return self.parent[u]
    def union(self, u, v):
        root_u = self.find(u)
        root_v = self.find(v)
        if root_u != root_v:
            self.parent[root_u] = root_v
    def is_same(self,u,v):
        return self.find(u) == self.find(v)

n = int(input())
uf = UnionFind(n)
for i in range(n):
    s,t = map(int,input().split())
    if uf.is_same(s,t):
        print(" ".join([str(s),str(t)]))
    else:
        uf.union(s,t)
```

## [109. 冗余连接II](https://www.programmercarl.com/kamacoder/0109.%E5%86%97%E4%BD%99%E8%BF%9E%E6%8E%A5II.html#%E6%80%9D%E8%B7%AF)

### 并查集
需要多回顾一下
```python
from collections import defaultdict
class UnionFind:
    def __init__(self,size):
        self.parent = list(range(size+1))
    def find(self,u):
        if u != self.parent[u]:
            self.parent[u] = self.find(self.parent[u])
        return self.parent[u]
    def union(self, u, v):
        root_u = self.find(u)
        root_v = self.find(v)
        if root_u != root_v:
            self.parent[root_u] = root_v
    def is_same(self,u,v):
        return self.find(u) == self.find(v)

def is_tree_after_remove_edge(edges, edge, n):
    global father
    father = UnionFind(n)
    for i in range(len(edges)):
        if i == edge:
            continue
        s, t = edges[i]
        if father.is_same(s,t):# 存在环
            return False
        else:
            father.union(s,t)
    return True
def get_remove_edge(edges):
    global father
    father = UnionFind(n)  
    for s,t in edges:
        if father.is_same(s,t):
            print(s,t)
            return
        else:
            father.union(s,t)
        

n = int(input())
edges = list()
in_degree = defaultdict(int)
for i in range(n):
    s,t = map(int,input().split())
    in_degree[t] += 1
    edges.append([s,t])
vec = list() 
# 寻找入度为2的节点，记录下标
for i in range(n-1, -1, -1):
    if in_degree[edges[i][1]] == 2:
        vec.append(i)
if len(vec) > 0:
    # 删除顺序靠后的边
    if is_tree_after_remove_edge(edges, vec[0], n):
        print(edges[vec[0]][0],edges[vec[0]][1])
    else:
    # 删除特定的边
        print(edges[vec[1]][0],edges[vec[1]][1])
else:# 有环存在
    get_remove_edge(edges)

```