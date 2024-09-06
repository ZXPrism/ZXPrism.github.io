---
date:
  created: 2024-09-06
---

# Dijkstra 与负权边

众所周知，Dijkstra 不可应用于含负权边的图上，因此我从来没有考虑过将它应用在负权图上的可能性。

但是，Dijkstra 并不是完全不能用于负权图——有时它可以产生正确的结果。

以下面这张图为例：

<center>
``` mermaid
graph LR
  A((0)) --> |7| B((1))
  B --> |—5| C((2))
  A --> |5| C
```
</center>

使用常见的、未经堆优化的 Dijkstra 算法（以节点 0 作为源点）：

```cpp linenums="1"
#include "bits/stdc++.h"

struct Edge {
    int v, w;
};

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);

    int n = 0, e = 0;
    std::cin >> n >> e;

    std::vector<std::vector<Edge>> adj(n);
    int u = 0, v = 0, w = 0;
    for (int i = 0; i < e; i++) {
        std::cin >> u >> v >> w;
        adj[u].emplace_back(v, w);
    }

    constexpr int inf = 0x3f3f3f3f;
    std::vector<int> dis(n, inf);
    std::vector<bool> vis(n);

    dis[0] = 0;

    for (int i = 0; i < n; i++) {
        int minDist = inf, k = -1;

        for (int j = 0; j < n; j++) {
            if (!vis[j] && dis[j] < minDist) {
                minDist = dis[j];
                k = j;
            }
        }
        vis[k] = 1;

        for (auto [v, w] : adj[k]) {
            if (dis[v] > dis[k] + w) {
                dis[v] = dis[k] + w;
            }
        }
    }

    for (int i = 0; i < n; i++) {
        std::cout << dis[i] << " \n"[i == n - 1];
    }

    return 0;
}
```

执行算法，得到节点 0 到其他节点的最短距离分别为：`0 7 2`，正确。

可以看到，Dijkstra 算法在某些负权图上也能得到正确的结果，尽管算法的执行过程中违背了 Dijkstra 算法本身的定义——一旦某个节点被访问（加入集合 S），源点到它的最短距离就已被确定。

问题在于，Dijkstra 并没有限制对于已访问节点的松弛操作，导致在后续访问新节点时，依然能对“理论上已确定最短距离”的节点进行最短距离的更新，尽管在非负权图上这种情况不可能出现。
