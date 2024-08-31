---
date:
  created: 2024-08-31
---

# Fenwick (BIT)
基础的树状数组，支持单点修改与区间和查询。

``` cpp linenums="1" title="Fenwick Code"
template <typename T>
class Fenwick {
public:
    Fenwick() = default;

    explicit Fenwick(int n) : _N(n) {
        _Data.resize(n + 1);
    }

    void Init(int n) {
        _N = n;
        _Data.resize(n + 1);
    }

    Fenwick(const std::vector<T> &v) {
        _N = v.size();
        _Data.resize(_N + 1);
        for (int i = 1, j = 0; i <= _N; i++) {
            _Data[i] += v[i - 1];
            j = i + _Lowbit(i);
            if (j <= _N)
                _Data[j] += _Data[i];
        }
    }

    [[nodiscard]] T QuerySum(int x) {
        T ans{};
        while (x) {
            ans += _Data[x];
            x -= _Lowbit(x);
        }
        return ans;
    }

    [[nodiscard]] T QueryRange(int l, int r) {
        return QuerySum(r) - QuerySum(l - 1);
    }

    void Add(int x, T k) {
        while (x <= _N) {
            _Data[x] += k;
            x += _Lowbit(x);
        }
    }

private:
    inline int _Lowbit(int x) {
        return x & -x;
    }

private:
    int _N;
    std::vector<T> _Data;
};
```
