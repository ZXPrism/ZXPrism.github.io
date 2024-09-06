---
date:
  created: 2024-08-31
---

# Custom Hash
防止 `std::unordered_map` 爆炸。

``` cpp linenums="1" title="Custom Hash Code"
// from Codeforces: @neal - https://codeforces.com/blog/entry/62393
struct custom_hash {
    static std::uint64_t splitmix64(std::uint64_t x) {
        // http://xorshift.di.unimi.it/splitmix64.c
        x += 0x9e3779b97f4a7c15;
        x = (x ^ (x >> 30)) * 0xbf58476d1ce4e5b9;
        x = (x ^ (x >> 27)) * 0x94d049bb133111eb;
        return x ^ (x >> 31);
    }

    size_t operator()(std::uint64_t x) const {
        static const std::uint64_t FIXED_RANDOM =
            std::chrono::steady_clock::now().time_since_epoch().count();
        return splitmix64(x + FIXED_RANDOM);
    }
};
```
