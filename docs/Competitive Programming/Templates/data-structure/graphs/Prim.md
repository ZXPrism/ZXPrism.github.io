---
date:
  created: 2024-09-02
---

# Prim

带堆（优先队列）优化的 Prim 算法，用于求解最小生成树，时间复杂度 $\mathcal{O}((n+e)\log{e})$ 。

根据时间复杂度可知，它在稠密图中表现不如 Kruskal 算法以及自己的 $\mathcal{O}(n^2)$ 暴力形式，所以优化并不适用于所有情况，一般建议求最小生成树统一使用 Kruskal 算法。

``` cpp linenums="1" title="Prim Code"
struct Edge {
    int v;
    int w;
};

bool operator<(const Edge &lhs, const Edge &rhs) {
    return lhs.w > rhs.w;
}

void solve() {
    int n = 0, m = 0;
    std::cin >> n >> m;

    std::vector<std::vector<Edge>> adj(n);
    for (int i = 0; i < m; i++) {
        int u = 0, v = 0, w = 0;
        std::cin >> u >> v >> w;
        --u, --v;
        adj[u].push_back({v, w});
        adj[v].push_back({u, w});
    }

    constexpr int inf = 0x3f3f3f3f;
    std::priority_queue<Edge> q;
    std::vector<int> dis(n, inf);
    std::vector<bool> vis(n);

    int cnt = 0, ans = 0;
    dis[0] = 0;
    q.push({0, 0});
    while (!q.empty()) {
        if (cnt == n) {
            break;
        }

        auto [u, w] = q.top();
        q.pop();

        if (vis[u]) {
            continue;
        }
        vis[u] = 1;
        ++cnt;
        ans += w;

        for (auto [v, w] : adj[u]) {
            if (w < dis[v]) {
                dis[v] = w;
                q.push({v, w});
            }
        }
    }

    if (cnt == n) {
        std::cout << ans << '\n';
    } else {
        std::cout << "not a connected graph\n";
    }
}
```
