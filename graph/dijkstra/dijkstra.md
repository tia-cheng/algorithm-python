# Dijkstra

## 题眼
* 计算从单一源点到其他所有节点的最短路径
* 有权重，而且权重>=0
* 求min time, cost
* 唯一的明确起点：“从node K发出信号”、“从src到dst”、“从网格左上角 (0,0) 走到右下角”

## 思路
* **本质:** 带有Priority Queue的 BFS。每次都从当前未访问的节点中，贪心地挑选距离起点“最近”的节点进行扩展。
* **Relaxation:** 通过中转节点寻找更短的路径。如果 `dist[起点 -> 中转节点] + weight(中转节点 -> 目标节点) < dist[起点 -> 目标节点]`，则更新已知最短距离。
* **限制:** **不能有负权**。因为 Dijkstra 基于贪心策略，一旦一个节点出堆，其最短路径就被认为已经收敛并确定。

## 模板

```python
from collections import defaultdict
import heapq

def dijkstra(n: int, edges: list[list[int]], start: int) -> dict:
    # 1. Adjacency List
    adj = defaultdict(list)
    for u, v, w in edges:
        adj[u].append((v, w))
        adj[v].append((u, w))

    # 2. 初始化距离表与pq
    # dist array只起到一个剪枝作用，不然heap就被塞爆了
    dist = [float('inf')] * n
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
```

## TC & SC
* $O((V + E) \log E)$ : logE: heappop, heappush

* $O(V + E)$ : adj: V+E. heap: E

# 带有额外限制的Dijkstra
* 加了限制条件: “最多只能经过 K 个 stops”，“最多可以消除 k 个障碍物”
* 限制条件(stops)做 Array, 优化目标(cost)做 Heap Sort Key
* Array只起到一个剪枝作用