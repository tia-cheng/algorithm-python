# Dijkstra 单源最短路径

## 思路
* **本质:** 带有优先队列 (Priority Queue) 的 BFS。每次都从当前未访问的节点中，贪心地挑选距离起点“最近”的节点进行扩展。
* **松弛 (Relaxation):** 通过中转节点寻找更短的路径。如果 `dist[起点 -> 中转节点] + weight(中转节点 -> 目标节点) < dist[起点 -> 目标节点]`，则更新已知最短距离。
* **限制:** 图中**不能包含负权边**。因为 Dijkstra 基于贪心策略，一旦一个节点出堆，其最短路径就被认为已经收敛并确定。

## 模板

```python
from collections import defaultdict
import heapq

def dijkstra(n: int, edges: list[list[int]], start: int) -> dict:
    # 1. 构建邻接表 (Adjacency List)
    adj = {i: [] for i in range(n)}
    for u, v, w in edges:
        adj[u].append((v, w))
        # graph[v].append((u, w))  # 若为undirected则两边都要加进去

    # 2. 初始化距离表与优先队列
    dist = [float('inf')] * n
    # or: dist = {i: float('inf') for i in range(n)}
    dist[start] = 0

    pq = [(0, start)]

    # 3. 贪心
    while pq:
        d, u = heapq.heappop(pq)

        # ⚠️ 关键剪枝 (Lazy Deletion): 
        # 堆中可能存有同一个节点的历史冗余记录，若取出的距离大于最新距离，直接作废
        # 只能大于，不能大于等于
        if d > dist[u]:
            continue

        for nei, w in graph[u]:
            new_d = d + w
            # 发现更短路径，进行松弛
            if new_d < dist[nei]:
                dist[nei] = new_d
                heapq.heappush(pq, (new_d, nei))

    return dist[end_node]