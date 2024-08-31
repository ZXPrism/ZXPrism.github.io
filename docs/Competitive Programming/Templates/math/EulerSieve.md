---
date:
  created: 2024-08-31
---

# Euler Sieve
使用 $O(n)$ 的时间预处理质数。

``` cpp linenums="1" title="Euler Sieve Code"
class EulerSieve {
public:
    EulerSieve() = default;

    explicit EulerSieve(int n) : _N(n) {
        _NotPrime.resize(n + 1);
    }

    void Init(int n) {
        _N = n;
        _NotPrime.resize(n + 1);
    }

    void Sieve() {
        _NotPrime[0] = 1;
        _NotPrime[1] = 1;

        for (int i = 2; i <= _N; i++) {
            if (!_NotPrime[i]) {
                _Primes.push_back(i);
            }

            for (auto p : _Primes) {
                if (i * p > _N) {
                    break;
                }

                _NotPrime[i * p] = 1;

                if (i % p == 0) {
                    break;
                }
            }
        }
    }

    [[nodiscard]] inline bool IsPrime(int x) {
        return !_NotPrime[x];
    }

    [[nodiscard]] inline int GetKthPrime(int k) {
        return _Primes[k - 1];
    }

private:
    int _N;
    std::vector<int> _NotPrime;
    std::vector<int> _Primes;
};
```
