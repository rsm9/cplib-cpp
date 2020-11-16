---
data:
  _extendedDependsOn:
  - icon: ':heavy_check_mark:'
    path: linear_algebra_matrix/linear_recurrence.hpp
    title: linear_algebra_matrix/linear_recurrence.hpp
  - icon: ':heavy_check_mark:'
    path: modint.hpp
    title: modint.hpp
  _extendedRequiredBy: []
  _extendedVerifiedWith: []
  _pathExtension: cpp
  _verificationStatusIcon: ':heavy_check_mark:'
  attributes:
    '*NOT_SPECIAL_COMMENTS*': ''
    PROBLEM: https://judge.yosupo.jp/problem/find_linear_recurrence
    links:
    - https://judge.yosupo.jp/problem/find_linear_recurrence
  bundledCode: "#line 2 \"linear_algebra_matrix/linear_recurrence.hpp\"\n#include\
    \ <algorithm>\n#include <cassert>\n#include <utility>\n#include <vector>\n\n//\
    \ CUT begin\n// Berlekamp\u2013Massey algorithm\n// <https://en.wikipedia.org/wiki/Berlekamp%E2%80%93Massey_algorithm>\n\
    // Complexity: O(N^2)\n// input: S = sequence from field K\n// return: L     \
    \     = degree of minimal polynomial,\n//         C_reversed = monic min. polynomial\
    \ (size = L + 1, reversed order, C_reversed[0] = 1))\n// Formula: convolve(S,\
    \ C_reversed)[i] = 0 for i >= L\n// Example:\n// - [1, 2, 4, 8, 16]   -> (1, [1,\
    \ -2])\n// - [1, 1, 2, 3, 5, 8] -> (2, [1, -1, -1])\n// - [0, 0, 0, 0, 1]    ->\
    \ (5, [1, 0, 0, 0, 0, 998244352]) (mod 998244353)\n// - []                 ->\
    \ (0, [1])\n// - [0, 0, 0]          -> (0, [1])\n// - [-2]               -> (1,\
    \ [1, 2])\ntemplate <typename Tfield>\nstd::pair<int, std::vector<Tfield>> linear_recurrence(const\
    \ std::vector<Tfield> &S)\n{\n    int N = S.size();\n    using poly = std::vector<Tfield>;\n\
    \    poly C_reversed{1}, B{1};\n    int L = 0, m = 1;\n    Tfield b = 1;\n\n \
    \   // adjust: C(x) <- C(x) - (d / b) x^m B(x)\n    auto adjust = [](poly C, const\
    \ poly &B, Tfield d, Tfield b, int m) -> poly {\n        C.resize(std::max(C.size(),\
    \ B.size() + m));\n        Tfield a = d / b;\n        for (unsigned i = 0; i <\
    \ B.size(); i++) C[i + m] -= a * B[i];\n        return C;\n    };\n\n    for (int\
    \ n = 0; n < N; n++) {\n        Tfield d = S[n];\n        for (int i = 1; i <=\
    \ L; i++) d += C_reversed[i] * S[n - i];\n\n        if (d == 0) m++;\n       \
    \ else if (2 * L <= n) {\n            poly T = C_reversed;\n            C_reversed\
    \ = adjust(C_reversed, B, d, b, m);\n            L = n + 1 - L;\n            B\
    \ = T;\n            b = d;\n            m = 1;\n        }\n        else C_reversed\
    \ = adjust(C_reversed, B, d, b, m++);\n    }\n    return std::make_pair(L, C_reversed);\n\
    }\n#line 2 \"modint.hpp\"\n#include <iostream>\n#line 4 \"modint.hpp\"\n#include\
    \ <set>\n\n// CUT begin\ntemplate <int mod>\nstruct ModInt\n{\n    using lint\
    \ = long long;\n    static int get_mod() { return mod; }\n    static int get_primitive_root()\
    \ {\n        static int primitive_root = 0;\n        if (!primitive_root) {\n\
    \            primitive_root = [&](){\n                std::set<int> fac;\n   \
    \             int v = mod - 1;\n                for (lint i = 2; i * i <= v; i++)\
    \ while (v % i == 0) fac.insert(i), v /= i;\n                if (v > 1) fac.insert(v);\n\
    \                for (int g = 1; g < mod; g++) {\n                    bool ok\
    \ = true;\n                    for (auto i : fac) if (ModInt(g).power((mod - 1)\
    \ / i) == 1) { ok = false; break; }\n                    if (ok) return g;\n \
    \               }\n                return -1;\n            }();\n        }\n \
    \       return primitive_root;\n    }\n    int val;\n    constexpr ModInt() :\
    \ val(0) {}\n    constexpr ModInt &_setval(lint v) { val = (v >= mod ? v - mod\
    \ : v); return *this; }\n    constexpr ModInt(lint v) { _setval(v % mod + mod);\
    \ }\n    explicit operator bool() const { return val != 0; }\n    constexpr ModInt\
    \ operator+(const ModInt &x) const { return ModInt()._setval((lint)val + x.val);\
    \ }\n    constexpr ModInt operator-(const ModInt &x) const { return ModInt()._setval((lint)val\
    \ - x.val + mod); }\n    constexpr ModInt operator*(const ModInt &x) const { return\
    \ ModInt()._setval((lint)val * x.val % mod); }\n    constexpr ModInt operator/(const\
    \ ModInt &x) const { return ModInt()._setval((lint)val * x.inv() % mod); }\n \
    \   constexpr ModInt operator-() const { return ModInt()._setval(mod - val); }\n\
    \    constexpr ModInt &operator+=(const ModInt &x) { return *this = *this + x;\
    \ }\n    constexpr ModInt &operator-=(const ModInt &x) { return *this = *this\
    \ - x; }\n    constexpr ModInt &operator*=(const ModInt &x) { return *this = *this\
    \ * x; }\n    constexpr ModInt &operator/=(const ModInt &x) { return *this = *this\
    \ / x; }\n    friend constexpr ModInt operator+(lint a, const ModInt &x) { return\
    \ ModInt()._setval(a % mod + x.val); }\n    friend constexpr ModInt operator-(lint\
    \ a, const ModInt &x) { return ModInt()._setval(a % mod - x.val + mod); }\n  \
    \  friend constexpr ModInt operator*(lint a, const ModInt &x) { return ModInt()._setval(a\
    \ % mod * x.val % mod); }\n    friend constexpr ModInt operator/(lint a, const\
    \ ModInt &x) { return ModInt()._setval(a % mod * x.inv() % mod); }\n    constexpr\
    \ bool operator==(const ModInt &x) const { return val == x.val; }\n    constexpr\
    \ bool operator!=(const ModInt &x) const { return val != x.val; }\n    bool operator<(const\
    \ ModInt &x) const { return val < x.val; }  // To use std::map<ModInt, T>\n  \
    \  friend std::istream &operator>>(std::istream &is, ModInt &x) { lint t; is >>\
    \ t; x = ModInt(t); return is; }\n    friend std::ostream &operator<<(std::ostream\
    \ &os, const ModInt &x) { os << x.val;  return os; }\n    constexpr lint power(lint\
    \ n) const {\n        lint ans = 1, tmp = this->val;\n        while (n) {\n  \
    \          if (n & 1) ans = ans * tmp % mod;\n            tmp = tmp * tmp % mod;\n\
    \            n /= 2;\n        }\n        return ans;\n    }\n    constexpr ModInt\
    \ pow(lint n) const {\n        return power(n);\n    }\n    constexpr lint inv()\
    \ const { return this->power(mod - 2); }\n    constexpr ModInt operator^(lint\
    \ n) const { return ModInt(this->power(n)); }\n    constexpr ModInt &operator^=(lint\
    \ n) { return *this = *this ^ n; }\n\n    inline ModInt fac() const {\n      \
    \  static std::vector<ModInt> facs;\n        int l0 = facs.size();\n        if\
    \ (l0 > this->val) return facs[this->val];\n\n        facs.resize(this->val +\
    \ 1);\n        for (int i = l0; i <= this->val; i++) facs[i] = (i == 0 ? ModInt(1)\
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
    \ = ModInt<998244353>;\n// using mint = ModInt<1000000007>;\n#line 3 \"linear_algebra_matrix/test/linear_recurrence.test.cpp\"\
    \n#define PROBLEM \"https://judge.yosupo.jp/problem/find_linear_recurrence\"\n\
    #line 5 \"linear_algebra_matrix/test/linear_recurrence.test.cpp\"\n\nusing mint\
    \ = ModInt<998244353>;\nint main()\n{\n    std::cin.tie(NULL);\n    std::ios::sync_with_stdio(false);\n\
    \    int N;\n    std::cin >> N;\n    std::vector<mint> A(N);\n    for (auto &a\
    \ : A)\n    {\n        std::cin >> a;\n    }\n    auto [L, poly] = linear_recurrence(A);\n\
    \    std::cout << L << '\\n';\n    for (int i = 1; i <= L; i++) std::cout << -poly[i]\
    \ << ' ';\n    std::cout << '\\n';\n}\n"
  code: "#include \"linear_algebra_matrix/linear_recurrence.hpp\"\n#include \"modint.hpp\"\
    \n#define PROBLEM \"https://judge.yosupo.jp/problem/find_linear_recurrence\"\n\
    #include <iostream>\n\nusing mint = ModInt<998244353>;\nint main()\n{\n    std::cin.tie(NULL);\n\
    \    std::ios::sync_with_stdio(false);\n    int N;\n    std::cin >> N;\n    std::vector<mint>\
    \ A(N);\n    for (auto &a : A)\n    {\n        std::cin >> a;\n    }\n    auto\
    \ [L, poly] = linear_recurrence(A);\n    std::cout << L << '\\n';\n    for (int\
    \ i = 1; i <= L; i++) std::cout << -poly[i] << ' ';\n    std::cout << '\\n';\n\
    }\n"
  dependsOn:
  - linear_algebra_matrix/linear_recurrence.hpp
  - modint.hpp
  isVerificationFile: true
  path: linear_algebra_matrix/test/linear_recurrence.test.cpp
  requiredBy: []
  timestamp: '2020-11-15 14:30:27+09:00'
  verificationStatus: TEST_ACCEPTED
  verifiedWith: []
documentation_of: linear_algebra_matrix/test/linear_recurrence.test.cpp
layout: document
redirect_from:
- /verify/linear_algebra_matrix/test/linear_recurrence.test.cpp
- /verify/linear_algebra_matrix/test/linear_recurrence.test.cpp.html
title: linear_algebra_matrix/test/linear_recurrence.test.cpp
---