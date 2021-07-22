---
data:
  _extendedDependsOn: []
  _extendedRequiredBy: []
  _extendedVerifiedWith:
  - icon: ':heavy_check_mark:'
    path: tree/test/diameter.test.cpp
    title: tree/test/diameter.test.cpp
  _isVerificationFailed: false
  _pathExtension: hpp
  _verificationStatusIcon: ':heavy_check_mark:'
  attributes:
    links: []
  bundledCode: "#line 2 \"tree/diameter.hpp\"\n#include <algorithm>\n#include <cassert>\n\
    #include <tuple>\n#include <utility>\n#include <vector>\n\n// CUT begin\n// \u6728\
    \u306E\u76F4\u5F84\u3068\uFF0C\u3053\u308C\u3092\u306A\u3059\u30D1\u30B9\u306E\
    \u9802\u70B9\u3092\u51FA\u529B\ntemplate <class Weight> struct TreeDiameter {\n\
    \    int n;\n    std::vector<std::vector<std::pair<int, Weight>>> to;\n    std::vector<int>\
    \ prev;\n    std::vector<Weight> dist;\n    TreeDiameter(int V) : n(V), to(V),\
    \ prev(V), dist(V) {}\n    void add_edge(int s, int t, Weight w) {\n        assert(0\
    \ <= s and s < n);\n        assert(0 <= t and t < n);\n        to[s].emplace_back(t,\
    \ w);\n        to[t].emplace_back(s, w);\n    }\n    int argdmax;\n    Weight\
    \ dmax;\n    void dfs(int now, int prv) {\n        if (dmax < dist[now]) dmax\
    \ = dist[now], argdmax = now;\n        for (auto p : to[now]) {\n            int\
    \ nxt;\n            Weight w;\n            std::tie(nxt, w) = p;\n           \
    \ if (nxt == prv) continue;\n            prev[nxt] = now;\n            dist[nxt]\
    \ = dist[now] + w;\n            dfs(nxt, now);\n        }\n    }\n    std::pair<Weight,\
    \ std::vector<int>> get_diameter_edges(int root = 0) {\n        prev[root] = -1;\n\
    \        dist[root] = 0;\n        dmax = 0, argdmax = root;\n        dfs(root,\
    \ -1);\n        dmax = 0, prev[argdmax] = -1, dist[argdmax] = 0;\n        dfs(argdmax,\
    \ -1);\n        std::vector<int> ret;\n        while (argdmax >= 0) {\n      \
    \      ret.push_back(argdmax);\n            argdmax = prev[argdmax];\n       \
    \ }\n        return {dmax, ret};\n    }\n};\n"
  code: "#pragma once\n#include <algorithm>\n#include <cassert>\n#include <tuple>\n\
    #include <utility>\n#include <vector>\n\n// CUT begin\n// \u6728\u306E\u76F4\u5F84\
    \u3068\uFF0C\u3053\u308C\u3092\u306A\u3059\u30D1\u30B9\u306E\u9802\u70B9\u3092\
    \u51FA\u529B\ntemplate <class Weight> struct TreeDiameter {\n    int n;\n    std::vector<std::vector<std::pair<int,\
    \ Weight>>> to;\n    std::vector<int> prev;\n    std::vector<Weight> dist;\n \
    \   TreeDiameter(int V) : n(V), to(V), prev(V), dist(V) {}\n    void add_edge(int\
    \ s, int t, Weight w) {\n        assert(0 <= s and s < n);\n        assert(0 <=\
    \ t and t < n);\n        to[s].emplace_back(t, w);\n        to[t].emplace_back(s,\
    \ w);\n    }\n    int argdmax;\n    Weight dmax;\n    void dfs(int now, int prv)\
    \ {\n        if (dmax < dist[now]) dmax = dist[now], argdmax = now;\n        for\
    \ (auto p : to[now]) {\n            int nxt;\n            Weight w;\n        \
    \    std::tie(nxt, w) = p;\n            if (nxt == prv) continue;\n          \
    \  prev[nxt] = now;\n            dist[nxt] = dist[now] + w;\n            dfs(nxt,\
    \ now);\n        }\n    }\n    std::pair<Weight, std::vector<int>> get_diameter_edges(int\
    \ root = 0) {\n        prev[root] = -1;\n        dist[root] = 0;\n        dmax\
    \ = 0, argdmax = root;\n        dfs(root, -1);\n        dmax = 0, prev[argdmax]\
    \ = -1, dist[argdmax] = 0;\n        dfs(argdmax, -1);\n        std::vector<int>\
    \ ret;\n        while (argdmax >= 0) {\n            ret.push_back(argdmax);\n\
    \            argdmax = prev[argdmax];\n        }\n        return {dmax, ret};\n\
    \    }\n};\n"
  dependsOn: []
  isVerificationFile: false
  path: tree/diameter.hpp
  requiredBy: []
  timestamp: '2021-06-09 00:31:07+09:00'
  verificationStatus: LIBRARY_ALL_AC
  verifiedWith:
  - tree/test/diameter.test.cpp
documentation_of: tree/diameter.hpp
layout: document
title: "Tree diameter \uFF08\u6728\u306E\u76F4\u5F84\uFF09"
---

木の直径を計算．

## 使用方法

```cpp
TreeDiameter<long long> tree(N);
for (auto [s, t, w] : edges) tree.add_edge(s, t, w);

// 頂点 r を含む部分木に関して，直径 D とこれをなす頂点列 vs （の一つ）を計算
auto [D, vs] = tree.get_diameter_edges(r);
```

## 問題例

- [Library Checker: Tree Diameter](https://judge.yosupo.jp/problem/tree_diameter)