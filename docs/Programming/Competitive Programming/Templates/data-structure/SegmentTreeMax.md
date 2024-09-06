---
date:
  created: 2024-09-06
---

# Segment Tree (Range Add + Range Max)
线段树，支持区间加与区间最大值查询。

一般的树状数组无法维护区间最值，因为取最值的操作不可逆，想要正确地更新最值，必须更新所有受影响的区间，重新计算各个父节点值，而线段树做的正是这样的事。

```cpp linenums="1"
template <typename T>
class SegmentTree {
public:
    explicit SegmentTree(int n) {
        _N = n;
        _Data.resize(_N << 2);
        _Tag.resize(_N << 2);
    }

    template <typename U>
    explicit SegmentTree(const std::vector<U> &v) {
        _N = v.size();
        _Data.resize(_N << 2);
        _Tag.resize(_N << 2);

        BuildTree(v, 1, _N, 1);
    }

    T QueryRange(int L, int R) {
        return _QueryRange(L, R, 1, _N, 1);
    }

    void Add(int L, int R, T k) {
        _Add(L, R, k, 1, _N, 1);
    }

private:
    template <typename U>
    void BuildTree(const std::vector<U> &v, int l, int r, int cur) {
        if (l == r) {
            _Data[cur] = v[l - 1];
            return;
        }

        int m = (l + r) >> 1;
        BuildTree(v, l, m, cur << 1);
        BuildTree(v, m + 1, r, (cur << 1) + 1);
        _Data[cur] = std::max(_Data[cur << 1], _Data[(cur << 1) + 1]);
    }

    T _QueryRange(int L, int R, int l, int r, int cur) {
        if (L <= l && r <= R) {
            return _Data[cur];
        }

        _Pushdown(l, r, cur);

        T ans = std::numeric_limits<T>::min();
        int m = (l + r) >> 1;
        if (L <= m) {
            ans = std::max(ans, _QueryRange(L, R, l, m, cur << 1));
        }
        if (R > m) {
            ans = std::max(ans, _QueryRange(L, R, m + 1, r, (cur << 1) + 1));
        }

        return ans;
    }

    void _Add(int L, int R, const T &k, int l, int r, int cur) {
        if (L <= l && r <= R) {
            _Data[cur] += k;
            _Tag[cur] += k;
            return;
        }

        _Pushdown(l, r, cur);

        int m = (l + r) >> 1;
        if (L <= m) {
            _Add(L, R, k, l, m, cur << 1);
        }
        if (R > m) {
            _Add(L, R, k, m + 1, r, (cur << 1) + 1);
        }
        _Data[cur] = std::max(_Data[cur << 1], _Data[(cur << 1) + 1]);
    }

    void _Pushdown(int l, int r, int cur) {
        if (_Tag[cur] && l != r) {
            int m = (l + r) >> 1;
            _Data[cur << 1] += _Tag[cur], _Data[(cur << 1) + 1] += _Tag[cur];
            _Tag[cur << 1] += _Tag[cur], _Tag[(cur << 1) + 1] += _Tag[cur];
            _Tag[cur] = 0;
        }
    }

private:
    int _N;
    std::vector<T> _Data;
    std::vector<T> _Tag;
};
```
