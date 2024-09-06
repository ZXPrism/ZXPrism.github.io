---
date:
  created: 2024-08-11
---

# Xor Sigma Problem
- 链接：[E - Xor Sigma Problem](https://atcoder.jp/contests/abc365/tasks/abc365_e)
- 难度：<font color="#008000">1095</font>
- 初见于： 2024/8/10 练习
---

## 分析
（待施工）

## 代码
```cpp linenums=1
void solve() {
    int n = 0;
    std::cin >> n;

    int mx = 0;
    std::vector<int> v(n);
    for (int i = 0; i < n; i++) {
        std::cin >> v[i];
        mx = std::max(mx, v[i]);
    }

    int nb = std::log2(mx) + 1;
    std::vector<i64> cnt(nb);
    for (int i = 0; i < nb; i++) {
        int pre = 0, prevb = 0;
        for (int j = 0; j < n; j++) {
            int cur = 0;

            if (j >= 1) {
                if (v[j] & 1) {
                    cur = j - 1 - pre;
                } else {
                    cur = pre;
                }
                cur += (v[j] & 1) ^ prevb;
            }

            cnt[i] += cur;
            pre = cur;
            prevb = v[j] & 1;
            v[j] >>= 1;
        }
    }

    i64 ans = 0, w = 1;
    for (int i = 0; i < nb; i++) {
        ans += w * cnt[i];
        w <<= 1;
    }

    std::cout << ans << '\n';
}
```

## 感悟
这道题真的难吗？我觉得不难，从做出之后的角度来说是这样，但我却花了几乎一天来解决这道题。

问题在哪里？无非是自己的思考深度不够。如果我的大脑以 DFS 算法来分析问题，那么其最大搜索深度很可能不超过 2。

我想天才们的大脑构造和常人不同，他们能在递归十几层的同时保证自己的思路清晰地跟进，直到得出可行的方案，而我做不到。我的大脑内存具有易失性，即便不掉电也会不定时且随机地丢失数据，导致递归无法继续下去，因而我很难思考复杂的逻辑。

但我没有想到一个极为简单的道理……可以用草稿纸作为外置内存。只是在草稿纸上简单模拟了一下样例的计算过程，就马上得到了思路，首先需要拆位处理，然后对每个元素的指定位做一次线性 DP。

提交后看到绿色的 AC 不禁笑着哭了……原来我也有这么一天能够写出这么 “高大上” 的程序。我成长了！！

不过看了别人的提交，代码比我都要短上许多，得好好学习一下。
