---
date:
  created: 2024-09-02
---

# Kruskal

Kruskal 算法，用于求解最小生成树，时间复杂度 $\mathcal{O}(e\log{e})$。

代码中的 `DSU` 类见本博客 `Competitive Programming / Templates / DataStructure` 下的 `DSU (Disjoint Set Union)` 一文。

``` cpp linenums="1" title="Kruskal Code"
struct Edge {
    int u, v, w;
};

void solve() {
    int n = 0, m = 0;
    std::cin >> n >> m;

    std::vector<Edge> edges(m);
    for (int i = 0; i < m; i++) {
        std::cin >> edges[i].u >> edges[i].v >> edges[i].w;
        --edges[i].u, --edges[i].v;
    }
    std::sort(edges.begin(), edges.end(),
              [](const Edge &lhs, const Edge &rhs) { return lhs.w < rhs.w; });

    bool ok = false;
    int ans = 0;
    DSU dsu(n);

    for (auto [u, v, w] : edges) {
        auto rU = dsu.Find(u), rV = dsu.Find(v);
        if (rU != rV) {
            dsu.Unite(rU, rV);
            ans += w;

            if (dsu.GetSize(rU) == n) {
                ok = true;
                break;
            }
        }
    }

    if (ok) {
        std::cout << ans << '\n';
    } else {
        std::cout << "orz\n";
    }
}
```
