---
date:
  created: 2024-08-31
---

# DSU (Disjoint Set Union)

基础的并查集，使用路径压缩与启发式合并。

``` cpp linenums="1" title="DSU Code"
class DSU {
public:
    explicit DSU(int n) {
        _Parent = std::vector<int>(n, -1);
    }

    void Unite(int x, int y) {
        x = Find(x);
        y = Find(y);
        if (x != y) {
            if (_Parent[x] < _Parent[y]) {
                _Parent[x] += _Parent[y];
                _Parent[y] = x;
            } else {
                _Parent[y] += _Parent[x];
                _Parent[x] = y;
            }
        }
    }

    [[nodiscard]] int Find(int x) {
        return _Parent[x] < 0 ? x : _Parent[x] = Find(_Parent[x]);
    }

    [[nodiscard]] bool Same(int x, int y) {
        return Find(x) == Find(y);
    }

    [[nodiscard]] int GetSize(int x) {
        return -_Parent[Find(x)];
    }

    void Print() {
        for (auto ele : _Parent) {
            std::cerr << ele << ' ';
        }
        std::cerr << '\n';
    }

private:
    std::vector<int> _Parent;
};
```
