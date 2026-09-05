# Floyd Warshall

## 题眼
* 多源最短路径，求任意两点之间最短距离
* weight可以是负的

## 思路
* 先建一个dist board，自己到自己是0，把edges里的权重丢进去
* 中转站、起点、终点，三层循环
* 其实就是DP
* 如果要求path, 可以再建一个爸爸board

## 模板

```python

class Solution:
    def floydWarshall(self, n: int, edges: List[List[int]]) -> int:
        dist = [[float('inf')] * n for _ in range(n)]
        
        for i in range(n):
            dist[i][i] = 0
            
        for u, v, w in edges:
            dist[u][v] = min(dist[u][v], w) 
            dist[v][u] = min(dist[v][u], w) 

        # 2. 核心三层循环（状态转移）
        for k in range(n):         # 中转点 k 必须在最外层
            for i in range(n):
                for j in range(n):
                    # 如果经 k 中转更近，则更新距离
                    if dist[i][k] + dist[k][j] < dist[i][j]:
                        dist[i][j] = dist[i][k] + dist[k][j]

        # 3. 负权环检测（视题目要求决定是否保留）
        # 如果走了一圈回到自己，距离竟然变成了负数，说明图中存在负权环
        for i in range(n):
            if dist[i][i] < 0:
                return -1 # 触发报错机制

        # 4. 根据题目需求处理并返回结果
        res = 0 
        
        return res

```

## TC & SC
* $O(V^3)$
* $O(V^2)$