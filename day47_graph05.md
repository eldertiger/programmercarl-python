# 图论 part5

## [寻找存在的路径](https://www.programmercarl.com/kamacoder/0107.%E5%AF%BB%E6%89%BE%E5%AD%98%E5%9C%A8%E7%9A%84%E8%B7%AF%E5%BE%84.html)

### 思路
使用并查集来表示节点的连通状态，查询时间复杂度O(1),空间复杂度O(n)。

```python
class UnionFind:
    def __init__(self,size):
        self.parent = list(range(size+1))
    def find(self,u):
        if self.parent[u] != u:
            self.parent[u] = self.find(self.parent[u])
        return self.parent[u]
    def union(self, u, v):
        root_u = self.find(u)
        root_v = self.find(v)
        if root_u != root_v:
            self.parent[root_v] = root_u
    def is_same(self, u, v):
        return self.find(u) == self.find(v)

n, m = map(int, input().split())
uf = UnionFind(n)
for _ in range(m):
    s, t = map(int, input().split())
    uf.union(s,t)
source, destination = map(int,input().split())
if uf.is_same(source, destination):
    print(1)
else:
    print(0)
    
```