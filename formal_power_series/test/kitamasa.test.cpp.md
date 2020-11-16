---
data:
  _extendedDependsOn:
  - icon: ':heavy_check_mark:'
    path: convolution/ntt.hpp
    title: convolution/ntt.hpp
  - icon: ':heavy_check_mark:'
    path: formal_power_series/monomial_mod_polynomial.hpp
    title: formal_power_series/monomial_mod_polynomial.hpp
  - icon: ':heavy_check_mark:'
    path: modint.hpp
    title: modint.hpp
  _extendedRequiredBy: []
  _extendedVerifiedWith: []
  _pathExtension: cpp
  _verificationStatusIcon: ':heavy_check_mark:'
  attributes:
    '*NOT_SPECIAL_COMMENTS*': ''
    PROBLEM: https://yukicoder.me/problems/no/214
    links:
    - https://yukicoder.me/problems/no/214
  bundledCode: "#line 1 \"formal_power_series/test/kitamasa.test.cpp\"\n#define PROBLEM\
    \ \"https://yukicoder.me/problems/no/214\"\n#line 2 \"modint.hpp\"\n#include <iostream>\n\
    #include <vector>\n#include <set>\n\n// CUT begin\ntemplate <int mod>\nstruct\
    \ ModInt\n{\n    using lint = long long;\n    static int get_mod() { return mod;\
    \ }\n    static int get_primitive_root() {\n        static int primitive_root\
    \ = 0;\n        if (!primitive_root) {\n            primitive_root = [&](){\n\
    \                std::set<int> fac;\n                int v = mod - 1;\n      \
    \          for (lint i = 2; i * i <= v; i++) while (v % i == 0) fac.insert(i),\
    \ v /= i;\n                if (v > 1) fac.insert(v);\n                for (int\
    \ g = 1; g < mod; g++) {\n                    bool ok = true;\n              \
    \      for (auto i : fac) if (ModInt(g).power((mod - 1) / i) == 1) { ok = false;\
    \ break; }\n                    if (ok) return g;\n                }\n       \
    \         return -1;\n            }();\n        }\n        return primitive_root;\n\
    \    }\n    int val;\n    constexpr ModInt() : val(0) {}\n    constexpr ModInt\
    \ &_setval(lint v) { val = (v >= mod ? v - mod : v); return *this; }\n    constexpr\
    \ ModInt(lint v) { _setval(v % mod + mod); }\n    explicit operator bool() const\
    \ { return val != 0; }\n    constexpr ModInt operator+(const ModInt &x) const\
    \ { return ModInt()._setval((lint)val + x.val); }\n    constexpr ModInt operator-(const\
    \ ModInt &x) const { return ModInt()._setval((lint)val - x.val + mod); }\n   \
    \ constexpr ModInt operator*(const ModInt &x) const { return ModInt()._setval((lint)val\
    \ * x.val % mod); }\n    constexpr ModInt operator/(const ModInt &x) const { return\
    \ ModInt()._setval((lint)val * x.inv() % mod); }\n    constexpr ModInt operator-()\
    \ const { return ModInt()._setval(mod - val); }\n    constexpr ModInt &operator+=(const\
    \ ModInt &x) { return *this = *this + x; }\n    constexpr ModInt &operator-=(const\
    \ ModInt &x) { return *this = *this - x; }\n    constexpr ModInt &operator*=(const\
    \ ModInt &x) { return *this = *this * x; }\n    constexpr ModInt &operator/=(const\
    \ ModInt &x) { return *this = *this / x; }\n    friend constexpr ModInt operator+(lint\
    \ a, const ModInt &x) { return ModInt()._setval(a % mod + x.val); }\n    friend\
    \ constexpr ModInt operator-(lint a, const ModInt &x) { return ModInt()._setval(a\
    \ % mod - x.val + mod); }\n    friend constexpr ModInt operator*(lint a, const\
    \ ModInt &x) { return ModInt()._setval(a % mod * x.val % mod); }\n    friend constexpr\
    \ ModInt operator/(lint a, const ModInt &x) { return ModInt()._setval(a % mod\
    \ * x.inv() % mod); }\n    constexpr bool operator==(const ModInt &x) const {\
    \ return val == x.val; }\n    constexpr bool operator!=(const ModInt &x) const\
    \ { return val != x.val; }\n    bool operator<(const ModInt &x) const { return\
    \ val < x.val; }  // To use std::map<ModInt, T>\n    friend std::istream &operator>>(std::istream\
    \ &is, ModInt &x) { lint t; is >> t; x = ModInt(t); return is; }\n    friend std::ostream\
    \ &operator<<(std::ostream &os, const ModInt &x) { os << x.val;  return os; }\n\
    \    constexpr lint power(lint n) const {\n        lint ans = 1, tmp = this->val;\n\
    \        while (n) {\n            if (n & 1) ans = ans * tmp % mod;\n        \
    \    tmp = tmp * tmp % mod;\n            n /= 2;\n        }\n        return ans;\n\
    \    }\n    constexpr ModInt pow(lint n) const {\n        return power(n);\n \
    \   }\n    constexpr lint inv() const { return this->power(mod - 2); }\n    constexpr\
    \ ModInt operator^(lint n) const { return ModInt(this->power(n)); }\n    constexpr\
    \ ModInt &operator^=(lint n) { return *this = *this ^ n; }\n\n    inline ModInt\
    \ fac() const {\n        static std::vector<ModInt> facs;\n        int l0 = facs.size();\n\
    \        if (l0 > this->val) return facs[this->val];\n\n        facs.resize(this->val\
    \ + 1);\n        for (int i = l0; i <= this->val; i++) facs[i] = (i == 0 ? ModInt(1)\
    \ : facs[i - 1] * ModInt(i));\n        return facs[this->val];\n    }\n\n    ModInt\
    \ doublefac() const {\n        lint k = (this->val + 1) / 2;\n        if (this->val\
    \ & 1) return ModInt(k * 2).fac() / ModInt(2).power(k) / ModInt(k).fac();\n  \
    \      else return ModInt(k).fac() * ModInt(2).power(k);\n    }\n\n    ModInt\
    \ nCr(const ModInt &r) const {\n        if (this->val < r.val) return ModInt(0);\n\
    \        return this->fac() / ((*this - r).fac() * r.fac());\n    }\n\n    ModInt\
    \ sqrt() const {\n        if (val == 0) return 0;\n        if (mod == 2) return\
    \ val;\n        if (power((mod - 1) / 2) != 1) return 0;\n        ModInt b = 1;\n\
    \        while (b.power((mod - 1) / 2) == 1) b += 1;\n        int e = 0, m = mod\
    \ - 1;\n        while (m % 2 == 0) m >>= 1, e++;\n        ModInt x = power((m\
    \ - 1) / 2), y = (*this) * x * x;\n        x *= (*this);\n        ModInt z = b.power(m);\n\
    \        while (y != 1) {\n            int j = 0;\n            ModInt t = y;\n\
    \            while (t != 1) j++, t *= t;\n            z = z.power(1LL << (e -\
    \ j - 1));\n            x *= z, z *= z, y *= z;\n            e = j;\n        }\n\
    \        return ModInt(std::min(x.val, mod - x.val));\n    }\n};\n// using mint\
    \ = ModInt<998244353>;\n// using mint = ModInt<1000000007>;\n#line 3 \"convolution/ntt.hpp\"\
    \n\n#include <algorithm>\n#include <array>\n#include <cassert>\n#include <tuple>\n\
    #line 9 \"convolution/ntt.hpp\"\n\n// CUT begin\n// Integer convolution for arbitrary\
    \ mod\n// with NTT (and Garner's algorithm) for ModInt / ModIntRuntime class.\n\
    // We skip Garner's algorithm if `skip_garner` is true or mod is in `nttprimes`.\n\
    // input: a (size: n), b (size: m)\n// return: vector (size: n + m - 1)\ntemplate\
    \ <typename MODINT>\nstd::vector<MODINT> nttconv(std::vector<MODINT> a, std::vector<MODINT>\
    \ b, bool skip_garner = false);\n\nconstexpr int nttprimes[3] = {998244353, 167772161,\
    \ 469762049};\n\n// Integer FFT (Fast Fourier Transform) for ModInt class\n//\
    \ (Also known as Number Theoretic Transform, NTT)\n// is_inverse: inverse transform\n\
    // ** Input size must be 2^n **\ntemplate <typename MODINT>\nvoid ntt(std::vector<MODINT>\
    \ &a, bool is_inverse = false)\n{\n    int n = a.size();\n    if (n == 1) return;\n\
    \    static const int mod = MODINT::get_mod();\n    static const MODINT root =\
    \ MODINT::get_primitive_root();\n    assert(__builtin_popcount(n) == 1 and (mod\
    \ - 1) % n == 0);\n\n    static std::vector<MODINT> w{1}, iw{1};\n    for (int\
    \ m = w.size(); m < n / 2; m *= 2) {\n        MODINT dw = root.power((mod - 1)\
    \ / (4 * m)), dwinv = 1 / dw;\n        w.resize(m * 2), iw.resize(m * 2);\n  \
    \      for (int i = 0; i < m; i++) w[m + i] = w[i] * dw, iw[m + i] = iw[i] * dwinv;\n\
    \    }\n\n    if (!is_inverse) {\n        for (int m = n; m >>= 1;) {\n      \
    \      for (int s = 0, k = 0; s < n; s += 2 * m, k++) {\n                for (int\
    \ i = s; i < s + m; i++) {\n                    MODINT x = a[i], y = a[i + m]\
    \ * w[k];\n                    a[i] = x + y, a[i + m] = x - y;\n             \
    \   }\n            }\n        }\n    }\n    else {\n        for (int m = 1; m\
    \ < n; m *= 2) {\n            for (int s = 0, k = 0; s < n; s += 2 * m, k++) {\n\
    \                for (int i = s; i < s + m; i++) {\n                    MODINT\
    \ x = a[i], y = a[i + m];\n                    a[i] = x + y, a[i + m] = (x - y)\
    \ * iw[k];\n                }\n            }\n        }\n        int n_inv = MODINT(n).inv();\n\
    \        for (auto &v : a) v *= n_inv;\n    }\n}\ntemplate <int MOD>\nstd::vector<ModInt<MOD>>\
    \ nttconv_(const std::vector<int> &a, const std::vector<int> &b) {\n    int sz\
    \ = a.size();\n    assert(a.size() == b.size() and __builtin_popcount(sz) == 1);\n\
    \    std::vector<ModInt<MOD>> ap(sz), bp(sz);\n    for (int i = 0; i < sz; i++)\
    \ ap[i] = a[i], bp[i] = b[i];\n    ntt(ap, false);\n    if (a == b) bp = ap;\n\
    \    else ntt(bp, false);\n    for (int i = 0; i < sz; i++) ap[i] *= bp[i];\n\
    \    ntt(ap, true);\n    return ap;\n}\nlong long garner_ntt_(int r0, int r1,\
    \ int r2, int mod)\n{\n    using mint2 = ModInt<nttprimes[2]>;\n    static const\
    \ long long m01 = 1LL * nttprimes[0] * nttprimes[1];\n    static const long long\
    \ m0_inv_m1 = ModInt<nttprimes[1]>(nttprimes[0]).inv();\n    static const long\
    \ long m01_inv_m2 = mint2(m01).inv();\n\n    int v1 = (m0_inv_m1 * (r1 + nttprimes[1]\
    \ - r0)) % nttprimes[1];\n    auto v2 = (mint2(r2) - r0 - mint2(nttprimes[0])\
    \ * v1) * m01_inv_m2;\n    return (r0 + 1LL * nttprimes[0] * v1 + m01 % mod *\
    \ v2.val) % mod;\n}\ntemplate <typename MODINT>\nstd::vector<MODINT> nttconv(std::vector<MODINT>\
    \ a, std::vector<MODINT> b, bool skip_garner)\n{\n    int sz = 1, n = a.size(),\
    \ m = b.size();\n    while (sz < n + m) sz <<= 1;\n    if (sz <= 16) {\n     \
    \   std::vector<MODINT> ret(n + m - 1);\n        for (int i = 0; i < n; i++) {\n\
    \            for (int j = 0; j < m; j++) ret[i + j] += a[i] * b[j];\n        }\n\
    \        return ret;\n    }\n    int mod = MODINT::get_mod();\n    if (skip_garner\
    \ or std::find(std::begin(nttprimes), std::end(nttprimes), mod) != std::end(nttprimes))\n\
    \    {\n        a.resize(sz), b.resize(sz);\n        if (a == b) { ntt(a, false);\
    \ b = a; }\n        else ntt(a, false), ntt(b, false);\n        for (int i = 0;\
    \ i < sz; i++) a[i] *= b[i];\n        ntt(a, true);\n        a.resize(n + m -\
    \ 1);\n    }\n    else {\n        std::vector<int> ai(sz), bi(sz);\n        for\
    \ (int i = 0; i < n; i++) ai[i] = a[i].val;\n        for (int i = 0; i < m; i++)\
    \ bi[i] = b[i].val;\n        auto ntt0 = nttconv_<nttprimes[0]>(ai, bi);\n   \
    \     auto ntt1 = nttconv_<nttprimes[1]>(ai, bi);\n        auto ntt2 = nttconv_<nttprimes[2]>(ai,\
    \ bi);\n        a.resize(n + m - 1);\n        for (int i = 0; i < n + m - 1; i++)\
    \ {\n            a[i] = garner_ntt_(ntt0[i].val, ntt1[i].val, ntt2[i].val, mod);\n\
    \        }\n    }\n    return a;\n}\n#line 4 \"formal_power_series/monomial_mod_polynomial.hpp\"\
    \n\n// CUT begin\n// Calculate x^N mod f(x)\n// Known as `Kitamasa method`\n//\
    \ Input: f_reversed: monic, reversed (f_reversed[0] = 1)\n// Complexity: O(K^2\
    \ lgN) (K: deg. of f)\n// Example: (4, [1, -1, -1]) -> [2, 3]\n//          ( x^4\
    \ = (x^2 + x + 2)(x^2 - x - 1) + 3x + 2 )\n// Reference: <http://misawa.github.io/others/fast_kitamasa_method.html>\n\
    //            <http://sugarknri.hatenablog.com/entry/2017/11/18/233936>\ntemplate\
    \ <typename Tfield>\nstd::vector<Tfield> monomial_mod_polynomial(long long N,\
    \ const std::vector<Tfield> &f_reversed)\n{\n    assert(!f_reversed.empty() and\
    \ f_reversed[0] == 1);\n    int K = f_reversed.size() - 1;\n    if (!K) return\
    \ {};\n    int D = 64 - __builtin_clzll(N);\n    std::vector<Tfield> ret(K, 0);\n\
    \    ret[0] = 1;\n    auto self_conv = [](std::vector<Tfield> x) -> std::vector<Tfield>\
    \ {\n        int d = x.size();\n        std::vector<Tfield> ret(d * 2 - 1);\n\
    \        for (int i = 0; i < d; i++)\n        {\n            ret[i * 2] += x[i]\
    \ * x[i];\n            for (int j = 0; j < i; j++) ret[i + j] += x[i] * x[j] *\
    \ 2;\n        }\n        return ret;\n    };\n    for (int d = D; d--;)\n    {\n\
    \        ret = self_conv(ret);\n        for (int i = 2 * K - 2; i >= K; i--)\n\
    \        {\n            for (int j = 1; j <= K; j++) ret[i - j] -= ret[i] * f_reversed[j];\n\
    \        }\n        ret.resize(K);\n        if ((N >> d) & 1)\n        {\n   \
    \         std::vector<Tfield> c(K);\n            c[0] = -ret[K - 1] * f_reversed[K];\n\
    \            for (int i = 1; i < K; i++)\n            {\n                c[i]\
    \ = ret[i - 1] - ret[K - 1] * f_reversed[K - i];\n            }\n            ret\
    \ = c;\n        }\n    }\n    return ret;\n}\n#line 5 \"formal_power_series/test/kitamasa.test.cpp\"\
    \nusing mint = ModInt<1000000007>;\n\n#line 8 \"formal_power_series/test/kitamasa.test.cpp\"\
    \n#include <numeric>\n\n\nstd::vector<mint> gen_dp(std::vector<int> v, int n)\n\
    {\n    std::vector<std::vector<mint>> dp(n + 1, std::vector<mint>(v.back() * n\
    \ + 1));\n    dp[0][0] = 1;\n    for (auto x : v)\n    {\n        for (int i =\
    \ n - 1; i >= 0; i--)\n        {\n            for (int j = 0; j < dp[i].size();\
    \ j++) if (dp[i][j])\n            {\n                for (int k = 1; i + k <=\
    \ n; k++) dp[i + k][j + x * k] += dp[i][j];\n            }\n        }\n    }\n\
    \    return dp.back();\n}\n\nint main()\n{\n    long long N;\n    int P, C;\n\
    \    std::cin >> N >> P >> C;\n    std::vector<mint> primes = gen_dp({2, 3, 5,\
    \ 7, 11, 13}, P), composites = gen_dp({4, 6, 8, 9, 10, 12}, C);\n    std::vector<mint>\
    \ f_reversed = nttconv(primes, composites);\n    std::vector<mint> dp(f_reversed.size());\n\
    \    dp[0] = 1;\n    for (int i = 0; i < dp.size(); i++)\n    {\n        for (int\
    \ j = 1; i + j < dp.size(); j++) dp[i + j] += dp[i] * f_reversed[j];\n    }\n\n\
    \    for (auto &x : f_reversed) x = -x;\n    f_reversed[0] = 1;\n\n    std::vector<mint>\
    \ g(f_reversed.size() - 1);\n    g[0] = 1;\n    if (N > f_reversed.size())\n \
    \   {\n        long long d = N - f_reversed.size();\n        N -= d;\n       \
    \ g = monomial_mod_polynomial<mint>(d, f_reversed);\n    }\n\n    auto prod_x\
    \ = [&](std::vector<mint> v) -> std::vector<mint> {\n        int K = v.size();\n\
    \        std::vector<mint> c(K);\n        c[0] = -v[K - 1] * f_reversed[K];\n\
    \        for (int i = 1; i < K; i++)\n        {\n            c[i] = v[i - 1] -\
    \ v[K - 1] * f_reversed[K - i];\n        }\n        return c;\n    };\n    mint\
    \ acc = 0;\n    for (int i = N; i < f_reversed.size(); i++) acc += f_reversed[i];\n\
    \    mint ret = 0;\n    while (N)\n    {\n        mint p = std::inner_product(g.begin(),\
    \ g.end(), dp.begin(), mint(0));\n        ret -= acc * p;\n        g = prod_x(g);\n\
    \        N--;\n        acc += f_reversed[N];\n    }\n    std::cout << ret << '\\\
    n';\n}\n"
  code: "#define PROBLEM \"https://yukicoder.me/problems/no/214\"\n#include \"modint.hpp\"\
    \n#include \"convolution/ntt.hpp\"\n#include \"formal_power_series/monomial_mod_polynomial.hpp\"\
    \nusing mint = ModInt<1000000007>;\n\n#include <iostream>\n#include <numeric>\n\
    \n\nstd::vector<mint> gen_dp(std::vector<int> v, int n)\n{\n    std::vector<std::vector<mint>>\
    \ dp(n + 1, std::vector<mint>(v.back() * n + 1));\n    dp[0][0] = 1;\n    for\
    \ (auto x : v)\n    {\n        for (int i = n - 1; i >= 0; i--)\n        {\n \
    \           for (int j = 0; j < dp[i].size(); j++) if (dp[i][j])\n           \
    \ {\n                for (int k = 1; i + k <= n; k++) dp[i + k][j + x * k] +=\
    \ dp[i][j];\n            }\n        }\n    }\n    return dp.back();\n}\n\nint\
    \ main()\n{\n    long long N;\n    int P, C;\n    std::cin >> N >> P >> C;\n \
    \   std::vector<mint> primes = gen_dp({2, 3, 5, 7, 11, 13}, P), composites = gen_dp({4,\
    \ 6, 8, 9, 10, 12}, C);\n    std::vector<mint> f_reversed = nttconv(primes, composites);\n\
    \    std::vector<mint> dp(f_reversed.size());\n    dp[0] = 1;\n    for (int i\
    \ = 0; i < dp.size(); i++)\n    {\n        for (int j = 1; i + j < dp.size();\
    \ j++) dp[i + j] += dp[i] * f_reversed[j];\n    }\n\n    for (auto &x : f_reversed)\
    \ x = -x;\n    f_reversed[0] = 1;\n\n    std::vector<mint> g(f_reversed.size()\
    \ - 1);\n    g[0] = 1;\n    if (N > f_reversed.size())\n    {\n        long long\
    \ d = N - f_reversed.size();\n        N -= d;\n        g = monomial_mod_polynomial<mint>(d,\
    \ f_reversed);\n    }\n\n    auto prod_x = [&](std::vector<mint> v) -> std::vector<mint>\
    \ {\n        int K = v.size();\n        std::vector<mint> c(K);\n        c[0]\
    \ = -v[K - 1] * f_reversed[K];\n        for (int i = 1; i < K; i++)\n        {\n\
    \            c[i] = v[i - 1] - v[K - 1] * f_reversed[K - i];\n        }\n    \
    \    return c;\n    };\n    mint acc = 0;\n    for (int i = N; i < f_reversed.size();\
    \ i++) acc += f_reversed[i];\n    mint ret = 0;\n    while (N)\n    {\n      \
    \  mint p = std::inner_product(g.begin(), g.end(), dp.begin(), mint(0));\n   \
    \     ret -= acc * p;\n        g = prod_x(g);\n        N--;\n        acc += f_reversed[N];\n\
    \    }\n    std::cout << ret << '\\n';\n}\n"
  dependsOn:
  - modint.hpp
  - convolution/ntt.hpp
  - formal_power_series/monomial_mod_polynomial.hpp
  isVerificationFile: true
  path: formal_power_series/test/kitamasa.test.cpp
  requiredBy: []
  timestamp: '2020-11-15 14:30:27+09:00'
  verificationStatus: TEST_ACCEPTED
  verifiedWith: []
documentation_of: formal_power_series/test/kitamasa.test.cpp
layout: document
redirect_from:
- /verify/formal_power_series/test/kitamasa.test.cpp
- /verify/formal_power_series/test/kitamasa.test.cpp.html
title: formal_power_series/test/kitamasa.test.cpp
---