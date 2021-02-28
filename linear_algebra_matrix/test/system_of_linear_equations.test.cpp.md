---
data:
  _extendedDependsOn:
  - icon: ':heavy_check_mark:'
    path: linear_algebra_matrix/linalg_modint.hpp
    title: linear_algebra_matrix/linalg_modint.hpp
  - icon: ':heavy_check_mark:'
    path: linear_algebra_matrix/system_of_linear_equations.hpp
    title: linear_algebra_matrix/system_of_linear_equations.hpp
  - icon: ':heavy_check_mark:'
    path: modint.hpp
    title: modint.hpp
  _extendedRequiredBy: []
  _extendedVerifiedWith: []
  _isVerificationFailed: false
  _pathExtension: cpp
  _verificationStatusIcon: ':heavy_check_mark:'
  attributes:
    '*NOT_SPECIAL_COMMENTS*': ''
    PROBLEM: https://judge.yosupo.jp/problem/system_of_linear_equations
    links:
    - https://judge.yosupo.jp/problem/system_of_linear_equations
  bundledCode: "#line 1 \"linear_algebra_matrix/test/system_of_linear_equations.test.cpp\"\
    \n#define PROBLEM \"https://judge.yosupo.jp/problem/system_of_linear_equations\"\
    \n#line 2 \"modint.hpp\"\n#include <iostream>\n#include <set>\n#include <vector>\n\
    \n// CUT begin\ntemplate <int mod> struct ModInt {\n#if __cplusplus >= 201402L\n\
    #define MDCONST constexpr\n#else\n#define MDCONST\n#endif\n    using lint = long\
    \ long;\n    MDCONST static int get_mod() { return mod; }\n    static int get_primitive_root()\
    \ {\n        static int primitive_root = 0;\n        if (!primitive_root) {\n\
    \            primitive_root = [&]() {\n                std::set<int> fac;\n  \
    \              int v = mod - 1;\n                for (lint i = 2; i * i <= v;\
    \ i++)\n                    while (v % i == 0) fac.insert(i), v /= i;\n      \
    \          if (v > 1) fac.insert(v);\n                for (int g = 1; g < mod;\
    \ g++) {\n                    bool ok = true;\n                    for (auto i\
    \ : fac)\n                        if (ModInt(g).pow((mod - 1) / i) == 1) {\n \
    \                           ok = false;\n                            break;\n\
    \                        }\n                    if (ok) return g;\n          \
    \      }\n                return -1;\n            }();\n        }\n        return\
    \ primitive_root;\n    }\n    int val;\n    MDCONST ModInt() : val(0) {}\n   \
    \ MDCONST ModInt &_setval(lint v) { return val = (v >= mod ? v - mod : v), *this;\
    \ }\n    MDCONST ModInt(lint v) { _setval(v % mod + mod); }\n    MDCONST explicit\
    \ operator bool() const { return val != 0; }\n    MDCONST ModInt operator+(const\
    \ ModInt &x) const { return ModInt()._setval((lint)val + x.val); }\n    MDCONST\
    \ ModInt operator-(const ModInt &x) const { return ModInt()._setval((lint)val\
    \ - x.val + mod); }\n    MDCONST ModInt operator*(const ModInt &x) const { return\
    \ ModInt()._setval((lint)val * x.val % mod); }\n    MDCONST ModInt operator/(const\
    \ ModInt &x) const { return ModInt()._setval((lint)val * x.inv() % mod); }\n \
    \   MDCONST ModInt operator-() const { return ModInt()._setval(mod - val); }\n\
    \    MDCONST ModInt &operator+=(const ModInt &x) { return *this = *this + x; }\n\
    \    MDCONST ModInt &operator-=(const ModInt &x) { return *this = *this - x; }\n\
    \    MDCONST ModInt &operator*=(const ModInt &x) { return *this = *this * x; }\n\
    \    MDCONST ModInt &operator/=(const ModInt &x) { return *this = *this / x; }\n\
    \    friend MDCONST ModInt operator+(lint a, const ModInt &x) { return ModInt()._setval(a\
    \ % mod + x.val); }\n    friend MDCONST ModInt operator-(lint a, const ModInt\
    \ &x) { return ModInt()._setval(a % mod - x.val + mod); }\n    friend MDCONST\
    \ ModInt operator*(lint a, const ModInt &x) { return ModInt()._setval(a % mod\
    \ * x.val % mod); }\n    friend MDCONST ModInt operator/(lint a, const ModInt\
    \ &x) { return ModInt()._setval(a % mod * x.inv() % mod); }\n    MDCONST bool\
    \ operator==(const ModInt &x) const { return val == x.val; }\n    MDCONST bool\
    \ operator!=(const ModInt &x) const { return val != x.val; }\n    MDCONST bool\
    \ operator<(const ModInt &x) const { return val < x.val; } // To use std::map<ModInt,\
    \ T>\n    friend std::istream &operator>>(std::istream &is, ModInt &x) {\n   \
    \     lint t;\n        return is >> t, x = ModInt(t), is;\n    }\n    MDCONST\
    \ friend std::ostream &operator<<(std::ostream &os, const ModInt &x) { return\
    \ os << x.val; }\n    MDCONST ModInt pow(lint n) const {\n        lint ans = 1,\
    \ tmp = this->val;\n        while (n) {\n            if (n & 1) ans = ans * tmp\
    \ % mod;\n            tmp = tmp * tmp % mod, n /= 2;\n        }\n        return\
    \ ans;\n    }\n\n    static std::vector<long long> facs, invs;\n    MDCONST static\
    \ void _precalculation(int N) {\n        int l0 = facs.size();\n        if (N\
    \ <= l0) return;\n        facs.resize(N), invs.resize(N);\n        for (int i\
    \ = l0; i < N; i++) facs[i] = facs[i - 1] * i % mod;\n        long long facinv\
    \ = ModInt(facs.back()).pow(mod - 2).val;\n        for (int i = N - 1; i >= l0;\
    \ i--) {\n            invs[i] = facinv * facs[i - 1] % mod;\n            facinv\
    \ = facinv * i % mod;\n        }\n    }\n    MDCONST lint inv() const {\n    \
    \    if (this->val < 1 << 20) {\n            while (this->val >= int(facs.size()))\
    \ _precalculation(facs.size() * 2);\n            return invs[this->val];\n   \
    \     } else {\n            return this->pow(mod - 2).val;\n        }\n    }\n\
    \    MDCONST ModInt fac() const {\n        while (this->val >= int(facs.size()))\
    \ _precalculation(facs.size() * 2);\n        return facs[this->val];\n    }\n\n\
    \    MDCONST ModInt doublefac() const {\n        lint k = (this->val + 1) / 2;\n\
    \        return (this->val & 1) ? ModInt(k * 2).fac() / (ModInt(2).pow(k) * ModInt(k).fac())\
    \ : ModInt(k).fac() * ModInt(2).pow(k);\n    }\n    MDCONST ModInt nCr(const ModInt\
    \ &r) const { return (this->val < r.val) ? 0 : this->fac() / ((*this - r).fac()\
    \ * r.fac()); }\n\n    ModInt sqrt() const {\n        if (val == 0) return 0;\n\
    \        if (mod == 2) return val;\n        if (pow((mod - 1) / 2) != 1) return\
    \ 0;\n        ModInt b = 1;\n        while (b.pow((mod - 1) / 2) == 1) b += 1;\n\
    \        int e = 0, m = mod - 1;\n        while (m % 2 == 0) m >>= 1, e++;\n \
    \       ModInt x = pow((m - 1) / 2), y = (*this) * x * x;\n        x *= (*this);\n\
    \        ModInt z = b.pow(m);\n        while (y != 1) {\n            int j = 0;\n\
    \            ModInt t = y;\n            while (t != 1) j++, t *= t;\n        \
    \    z = z.pow(1LL << (e - j - 1));\n            x *= z, z *= z, y *= z;\n   \
    \         e = j;\n        }\n        return ModInt(std::min(x.val, mod - x.val));\n\
    \    }\n};\ntemplate <int mod> std::vector<long long> ModInt<mod>::facs = {1};\n\
    template <int mod> std::vector<long long> ModInt<mod>::invs = {0};\n\n// using\
    \ mint = ModInt<998244353>;\n// using mint = ModInt<1000000007>;\n#line 2 \"linear_algebra_matrix/linalg_modint.hpp\"\
    \n#include <algorithm>\n#include <cassert>\n#line 5 \"linear_algebra_matrix/linalg_modint.hpp\"\
    \n#include <iterator>\n#line 7 \"linear_algebra_matrix/linalg_modint.hpp\"\n\n\
    // CUT begin\ntemplate <typename T> struct matrix {\n    int H, W;\n    std::vector<T>\
    \ elem;\n    typename std::vector<T>::iterator operator[](int i) { return elem.begin()\
    \ + i * W; }\n    inline T &at(int i, int j) { return elem[i * W + j]; }\n   \
    \ inline T get(int i, int j) const { return elem[i * W + j]; }\n    operator std::vector<std::vector<T>>()\
    \ const {\n        std::vector<std::vector<T>> ret(H);\n        for (int i = 0;\
    \ i < H; i++)\n            std::copy(elem.begin() + i * W, elem.begin() + (i +\
    \ 1) * W, std::back_inserter(ret[i]));\n        return ret;\n    }\n\n    matrix()\
    \ = default;\n    matrix(int H, int W) : H(H), W(W), elem(H * W) {}\n    matrix(const\
    \ std::vector<std::vector<T>> &d) : H(d.size()), W(d.size() ? d[0].size() : 0)\
    \ {\n        for (auto &raw : d) std::copy(raw.begin(), raw.end(), std::back_inserter(elem));\n\
    \    }\n\n    static matrix Identity(int N) {\n        matrix ret(N, N);\n   \
    \     for (int i = 0; i < N; i++) ret.at(i, i) = 1;\n        return ret;\n   \
    \ }\n\n    matrix operator-() const {\n        matrix ret(H, W);\n        for\
    \ (int i = 0; i < H * W; i++) ret.elem[i] = -elem[i];\n        return ret;\n \
    \   }\n    matrix operator*(const T &v) const {\n        matrix ret = *this;\n\
    \        for (auto &x : ret.elem) x *= v;\n        return ret;\n    }\n    matrix\
    \ operator/(const T &v) const {\n        matrix ret = *this;\n        for (auto\
    \ &x : ret.elem) x /= v;\n        return ret;\n    }\n    matrix operator+(const\
    \ matrix &r) const {\n        matrix ret = *this;\n        for (int i = 0; i <\
    \ H * W; i++) ret.elem[i] += r.elem[i];\n        return ret;\n    }\n    matrix\
    \ operator-(const matrix &r) const {\n        matrix ret = *this;\n        for\
    \ (int i = 0; i < H * W; i++) ret.elem[i] -= r.elem[i];\n        return ret;\n\
    \    }\n    matrix operator*(const matrix &r) const {\n        matrix ret(H, r.W);\n\
    \        for (int i = 0; i < H; i++) {\n            for (int k = 0; k < W; k++)\
    \ {\n                for (int j = 0; j < r.W; j++) { ret.at(i, j) += this->get(i,\
    \ k) * r.get(k, j); }\n            }\n        }\n        return ret;\n    }\n\
    \    matrix &operator*=(const T &v) { return *this = *this * v; }\n    matrix\
    \ &operator/=(const T &v) { return *this = *this / v; }\n    matrix &operator+=(const\
    \ matrix &r) { return *this = *this + r; }\n    matrix &operator-=(const matrix\
    \ &r) { return *this = *this - r; }\n    matrix &operator*=(const matrix &r) {\
    \ return *this = *this * r; }\n    bool operator==(const matrix &r) const { return\
    \ H == r.H and W == r.W and elem == r.elem; }\n    bool operator!=(const matrix\
    \ &r) const { return H != r.H or W != r.W or elem != r.elem; }\n    bool operator<(const\
    \ matrix &r) const { return elem < r.elem; }\n    matrix pow(int64_t n) const\
    \ {\n        matrix ret = Identity(H);\n        if (n == 0) return ret;\n    \
    \    for (int i = 63 - __builtin_clzll(n); i >= 0; i--) {\n            ret *=\
    \ ret;\n            if ((n >> i) & 1) ret *= (*this);\n        }\n        return\
    \ ret;\n    }\n    matrix transpose() const {\n        matrix ret(W, H);\n   \
    \     for (int i = 0; i < H; i++)\n            for (int j = 0; j < W; j++) ret.at(j,\
    \ i) = this->get(i, j);\n        return ret;\n    }\n    // Gauss-Jordan elimination\n\
    \    // - Require inverse for every non-zero element\n    // - Complexity: O(H^2\
    \ W)\n    matrix gauss_jordan() const {\n        int c = 0;\n        matrix mtr(*this);\n\
    \        std::vector<int> ws;\n        ws.reserve(W);\n        for (int h = 0;\
    \ h < H; h++) {\n            if (c == W) break;\n            int piv = -1;\n \
    \           for (int j = h; j < H; j++)\n                if (mtr.get(j, c)) {\n\
    \                    piv = j;\n                    break;\n                }\n\
    \            if (piv == -1) {\n                c++;\n                h--;\n  \
    \              continue;\n            }\n            if (h != piv) {\n       \
    \         for (int w = 0; w < W; w++) {\n                    std::swap(mtr[piv][w],\
    \ mtr[h][w]);\n                    mtr.at(piv, w) *= -1; // To preserve sign of\
    \ determinant\n                }\n            }\n            ws.clear();\n   \
    \         for (int w = c; w < W; w++) {\n                if (mtr.at(h, w) != 0)\
    \ ws.emplace_back(w);\n            }\n            const T hcinv = mtr.at(h, c).inv();\n\
    \            for (int hh = 0; hh < H; hh++)\n                if (hh != h) {\n\
    \                    const T coeff = mtr.at(hh, c) * hcinv;\n                \
    \    for (auto w : ws) mtr.at(hh, w) -= mtr.at(h, w) * coeff;\n              \
    \  }\n            c++;\n        }\n        return mtr;\n    }\n    int rank_of_gauss_jordan()\
    \ const {\n        for (int i = H * W - 1; i >= 0; i--)\n            if (elem[i])\
    \ return i / W + 1;\n        return 0;\n    }\n    T determinant_of_upper_triangle()\
    \ const {\n        T ret = 1;\n        for (int i = 0; i < H; i++) ret *= get(i,\
    \ i);\n        return ret;\n    }\n    int inverse() {\n        assert(H == W);\n\
    \        std::vector<std::vector<T>> ret = Identity(H), tmp = *this;\n       \
    \ int rank = 0;\n        for (int i = 0; i < H; i++) {\n            int ti = i;\n\
    \            while (ti < H and tmp[ti][i] == 0) ti++;\n            if (ti == H)\n\
    \                continue;\n            else\n                rank++;\n      \
    \      ret[i].swap(ret[ti]), tmp[i].swap(tmp[ti]);\n            T inv = tmp[i][i].inv();\n\
    \            for (int j = 0; j < W; j++) { ret[i][j] *= inv; }\n            for\
    \ (int j = i + 1; j < W; j++) { tmp[i][j] *= inv; }\n            for (int h =\
    \ 0; h < H; h++) {\n                if (i == h) continue;\n                const\
    \ T c = -tmp[h][i];\n                for (int j = 0; j < W; j++) { ret[h][j] +=\
    \ ret[i][j] * c; }\n                for (int j = i + 1; j < W; j++) { tmp[h][j]\
    \ += tmp[i][j] * c; }\n            }\n        }\n        *this = ret;\n      \
    \  return rank;\n    }\n    friend std::vector<T> operator*(const matrix &m, const\
    \ std::vector<T> &v) {\n        assert(m.W == int(v.size()));\n        std::vector<T>\
    \ ret(m.H);\n        for (int i = 0; i < m.H; i++) {\n            for (int j =\
    \ 0; j < m.W; j++) { ret[i] += m.get(i, j) * v[j]; }\n        }\n        return\
    \ ret;\n    }\n    friend std::vector<T> operator*(const std::vector<T> &v, const\
    \ matrix &m) {\n        assert(int(v.size()) == m.H);\n        std::vector<T>\
    \ ret(m.W);\n        for (int i = 0; i < m.H; i++) {\n            for (int j =\
    \ 0; j < m.W; j++) { ret[j] += v[i] * m.get(i, j); }\n        }\n        return\
    \ ret;\n    }\n    friend std::ostream &operator<<(std::ostream &os, const matrix\
    \ &x) {\n        os << \"[(\" << x.H << \" * \" << x.W << \" matrix)\";\n    \
    \    os << \"\\n[column sums: \";\n        for (int j = 0; j < x.W; j++) {\n \
    \           T s = 0;\n            for (int i = 0; i < x.H; i++) s += x.get(i,\
    \ j);\n            os << s << \",\";\n        }\n        os << \"]\";\n      \
    \  for (int i = 0; i < x.H; i++) {\n            os << \"\\n[\";\n            for\
    \ (int j = 0; j < x.W; j++) os << x.get(i, j) << \",\";\n            os << \"\
    ]\";\n        }\n        os << \"]\\n\";\n        return os;\n    }\n    friend\
    \ std::istream &operator>>(std::istream &is, matrix &x) {\n        for (auto &v\
    \ : x.elem) is >> v;\n        return is;\n    }\n};\n\n// Fibonacci numbers f(n)\
    \ = af(n - 1) + bf(n - 2)\n// Example (a = b = 1): 0=>1, 1=>1, 2=>2, 3=>3, 4=>5,\
    \ ...\ntemplate <typename T> T Fibonacci(long long int k, int a = 1, int b = 1)\
    \ {\n    matrix<T> mat(2, 2);\n    mat[0][1] = 1;\n    mat[1][0] = b;\n    mat[1][1]\
    \ = a;\n    return mat.pow(k + 1)[0][1];\n}\n#line 3 \"linear_algebra_matrix/system_of_linear_equations.hpp\"\
    \n#include <utility>\n#line 5 \"linear_algebra_matrix/system_of_linear_equations.hpp\"\
    \n\n// CUT begin\n// Solve Ax = b for T = ModInt<PRIME>\n// - retval: {one of\
    \ the solution, {freedoms}} (if solution exists)\n//           {{}, {}} (otherwise)\n\
    // Complexity:\n// - Yield one of the possible solutions: O(H^2 W) (H: # of eqs.,\
    \ W: # of variables)\n// - Enumerate all of the bases: O(HW(H + W))\ntemplate\
    \ <typename T>\nstd::pair<std::vector<T>, std::vector<std::vector<T>>> system_of_linear_equations(matrix<T>\
    \ A, std::vector<T> b) {\n    int H = A.H, W = A.W;\n    matrix<T> M(H, W + 1);\n\
    \    for (int i = 0; i < H; i++) {\n        for (int j = 0; j < W; j++) M[i][j]\
    \ = A[i][j];\n        M[i][W] = b[i];\n    }\n    M = M.gauss_jordan();\n    std::vector<int>\
    \ ss(W, -1);\n    for (int i = 0; i < H; i++) {\n        int j = 0;\n        while\
    \ (j <= W and M[i][j] == 0) j++;\n        if (j == W) { // No solution\n     \
    \       return {{}, {}};\n        }\n        if (j < W) ss[j] = i;\n    }\n  \
    \  std::vector<T> x(W);\n    std::vector<std::vector<T>> D;\n    for (int j =\
    \ 0; j < W; j++) {\n        if (ss[j] == -1) {\n            std::vector<T> d(W);\n\
    \            d[j] = 1;\n            for (int jj = 0; jj < j; jj++)\n         \
    \       if (ss[jj] != -1) d[jj] = -M[ss[jj]][j] / M[ss[jj]][jj];\n           \
    \ D.emplace_back(d);\n        } else\n            x[j] = M[ss[j]][W] / M[ss[j]][j];\n\
    \    }\n    return std::make_pair(x, D);\n}\n#line 5 \"linear_algebra_matrix/test/system_of_linear_equations.test.cpp\"\
    \nusing mint = ModInt<998244353>;\n\nint main() {\n    int N, M;\n    std::cin\
    \ >> N >> M;\n    matrix<mint> A(N, M);\n    std::cin >> A;\n    std::vector<mint>\
    \ b(N);\n    for (auto &x : b) std::cin >> x;\n    auto ret = system_of_linear_equations(A,\
    \ b);\n    if (ret.first.empty())\n        std::cout << -1 << \"\\n\";\n    else\
    \ {\n        std::cout << ret.second.size() << \"\\n\";\n        for (auto x :\
    \ ret.first) std::cout << x << \" \";\n        std::cout << \"\\n\";\n\n     \
    \   for (auto vec : ret.second) {\n            for (auto e : vec) std::cout <<\
    \ e << \" \";\n            std::cout << \"\\n\";\n        }\n    }\n}\n"
  code: "#define PROBLEM \"https://judge.yosupo.jp/problem/system_of_linear_equations\"\
    \n#include \"../../modint.hpp\"\n#include \"../system_of_linear_equations.hpp\"\
    \n#include <iostream>\nusing mint = ModInt<998244353>;\n\nint main() {\n    int\
    \ N, M;\n    std::cin >> N >> M;\n    matrix<mint> A(N, M);\n    std::cin >> A;\n\
    \    std::vector<mint> b(N);\n    for (auto &x : b) std::cin >> x;\n    auto ret\
    \ = system_of_linear_equations(A, b);\n    if (ret.first.empty())\n        std::cout\
    \ << -1 << \"\\n\";\n    else {\n        std::cout << ret.second.size() << \"\\\
    n\";\n        for (auto x : ret.first) std::cout << x << \" \";\n        std::cout\
    \ << \"\\n\";\n\n        for (auto vec : ret.second) {\n            for (auto\
    \ e : vec) std::cout << e << \" \";\n            std::cout << \"\\n\";\n     \
    \   }\n    }\n}\n"
  dependsOn:
  - modint.hpp
  - linear_algebra_matrix/system_of_linear_equations.hpp
  - linear_algebra_matrix/linalg_modint.hpp
  isVerificationFile: true
  path: linear_algebra_matrix/test/system_of_linear_equations.test.cpp
  requiredBy: []
  timestamp: '2021-01-10 04:11:55+09:00'
  verificationStatus: TEST_ACCEPTED
  verifiedWith: []
documentation_of: linear_algebra_matrix/test/system_of_linear_equations.test.cpp
layout: document
redirect_from:
- /verify/linear_algebra_matrix/test/system_of_linear_equations.test.cpp
- /verify/linear_algebra_matrix/test/system_of_linear_equations.test.cpp.html
title: linear_algebra_matrix/test/system_of_linear_equations.test.cpp
---