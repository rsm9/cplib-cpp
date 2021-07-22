---
data:
  _extendedDependsOn:
  - icon: ':heavy_check_mark:'
    path: data_structure/fibonacci_heap.hpp
    title: data_structure/fibonacci_heap.hpp
  _extendedRequiredBy: []
  _extendedVerifiedWith: []
  _isVerificationFailed: false
  _pathExtension: cpp
  _verificationStatusIcon: ':heavy_check_mark:'
  attributes:
    '*NOT_SPECIAL_COMMENTS*': ''
    PROBLEM: https://judge.yosupo.jp/problem/shortest_path
    links:
    - https://judge.yosupo.jp/problem/shortest_path
  bundledCode: "#line 1 \"data_structure/test/fibonacci_heap_shortestpath.test.cpp\"\
    \n#define PROBLEM \"https://judge.yosupo.jp/problem/shortest_path\"\n\n#line 2\
    \ \"data_structure/fibonacci_heap.hpp\"\n#include <array>\n#include <cassert>\n\
    #include <iostream>\n#include <list>\n\n// CUT begin\n// Fibonacci heap\n// -\
    \ Complexity:\n//   - empty(), size(), top(), push(), meld(): O(1)\n//   - pop(),\
    \ decrease(): O(lg N) amortized\n// - Reference:\n//   - \"Introduction to Algorithms,\
    \ Third Edition\", Chapter 19\n//   - <https://www.cs.princeton.edu/~wayne/teaching/fibonacci-heap.pdf>\n\
    //   - <https://rsk0315.hatenablog.com/entry/2019/10/29/151823>\ntemplate <typename\
    \ Tp> struct fibonacci_heap {\n    struct Node {\n        Tp val;\n        int\
    \ deg;\n        Node *parent, *left, *right, *child;\n        bool mark;\n   \
    \     Node() = default;\n        Node(Node const &) = default;\n        Node(Node\
    \ &&) = default;\n        Node &operator=(Node const &) = default;\n        Node\
    \ &operator=(Node &&) = default;\n        Node(Tp v) : val(v), deg(0), parent(nullptr),\
    \ left(nullptr), right(nullptr), child(nullptr), mark(false) {}\n        friend\
    \ std::ostream &operator<<(std::ostream &os, const Node &n) {\n            os\
    \ << '(' << n.val << ',';\n            if (n.child != nullptr) os << *(n.child)\
    \ << ',';\n            Node *now = n.right;\n            while (now != &n) {\n\
    \                os << now->val << ',';\n                if (now->child != nullptr)\
    \ os << *now->child << ',';\n                now = now->right;\n            }\n\
    \            os << ')';\n            return os;\n        }\n    };\n\n    int\
    \ sz;\n    std::list<Node *> roots;\n    Node *ptop;\n    inline void _chmin(Node\
    \ *cand) noexcept {\n        if (ptop == nullptr or cand->val < ptop->val) ptop\
    \ = cand;\n    }\n    fibonacci_heap() : sz(0), roots({}), ptop(nullptr) {}\n\n\
    \    bool empty() const noexcept { return sz == 0; }\n    int size() const noexcept\
    \ { return sz; }\n\n    std::array<Node *, 30> _arr;\n    void _fmerge(Node *ptr)\
    \ {\n        int d = ptr->deg;\n        if (_arr[d] == nullptr)\n            _arr[d]\
    \ = ptr;\n        else {\n            Node *cptr = _arr[d];\n            if (cptr->val\
    \ < ptr->val) std::swap(ptr, cptr);\n            ptr->deg++;\n            cptr->parent\
    \ = ptr;\n            if (ptr->child == nullptr)\n                ptr->child =\
    \ cptr;\n            else {\n                Node *cl = ptr->child, *cr = ptr->child->right;\n\
    \                assert(cl->right == cr and cr->left == cl);\n               \
    \ cptr->left = cl, cptr->right = cr, cl->right = cr->left = cptr;\n          \
    \  }\n            _arr[d] = nullptr;\n            _fmerge(ptr);\n        }\n \
    \   }\n    void _consolidate() {\n        _arr.fill(nullptr);\n        for (auto\
    \ ptr : roots)\n            if (ptr != nullptr) {\n                if (ptr->deg\
    \ < 0)\n                    delete ptr;\n                else\n              \
    \      _fmerge(ptr);\n            }\n        roots.clear(), ptop = nullptr;\n\
    \        for (auto ptr : _arr)\n            if (ptr != nullptr) _add_tree(ptr);\n\
    \    }\n\n    void _add_tree(Node *root) noexcept {\n        root->parent = nullptr;\n\
    \        root->left = root->right = root;\n        roots.emplace_back(root);\n\
    \        _chmin(root);\n    }\n\n    Node *push(const Tp &val) noexcept {\n  \
    \      sz++;\n        Node *ptr = new Node(val);\n        _add_tree(ptr);\n  \
    \      return ptr;\n    }\n\n    void meld(fibonacci_heap &&hp) {\n        sz\
    \ += hp.sz;\n        roots.splice(roots.end(), hp.roots);\n        if (hp.ptop\
    \ != nullptr) _chmin(hp.ptop);\n    }\n\n    void pop() {\n        assert(sz >\
    \ 0);\n        sz--;\n        Node *ch1 = ptop->child;\n        if (ch1 != nullptr)\
    \ {\n            Node *now = ch1;\n            while (true) {\n              \
    \  Node *nxt = now->right;\n                _add_tree(now);\n                now\
    \ = nxt;\n                if (now == ch1) break;\n            }\n        }\n \
    \       ptop->deg = -1;\n        _consolidate();\n    }\n\n    void _deldfs(Node\
    \ *now) {\n        while (now != nullptr) {\n            if (now->child != nullptr)\
    \ _deldfs(now->child);\n            Node *nxt = now->right;\n            delete\
    \ now;\n            now = nxt;\n        }\n    }\n    void clear() {\n       \
    \ for (auto root : roots) _deldfs(root);\n        sz = 0;\n        roots.clear();\n\
    \        ptop = nullptr;\n    }\n\n    void _cut(Node *x) noexcept {\n       \
    \ Node *y = x->parent;\n        assert(y != nullptr and y->deg > 0);\n       \
    \ Node *xr = x->right, *xl = x->left;\n        if (x == xr) {\n            y->child\
    \ = nullptr;\n        } else {\n            y->child = xr;\n            xl->right\
    \ = xr, xr->left = xl;\n        }\n        y->deg--;\n        _add_tree(x);\n\
    \        x->mark = false;\n    }\n    void _cascading_cut(Node *now) noexcept\
    \ {\n        assert(now != nullptr);\n        Node *par = now->parent;\n     \
    \   if (par == nullptr) return;\n        if (!now->mark)\n            now->mark\
    \ = true;\n        else {\n            _cut(now);\n            _cascading_cut(par);\n\
    \        }\n    }\n    void erase(Node *r) {\n        if (r->parent != nullptr)\
    \ {\n            Node *rpar = r->parent;\n            _cut(r);\n            _cascading_cut(rpar);\n\
    \        }\n        ptop = r;\n        pop();\n    }\n    bool decrease(Node *r,\
    \ const Tp new_val) {\n        assert(r != nullptr);\n        if (!(new_val <\
    \ r->val)) return false;\n        r->val = new_val;\n        if (r->parent !=\
    \ nullptr and new_val < r->parent->val) {\n            Node *rpar = r->parent;\n\
    \            _cut(r);\n            _cascading_cut(rpar);\n        }\n        _chmin(r);\n\
    \        return true;\n    }\n    Tp top() const {\n        assert(ptop != nullptr);\n\
    \        return ptop->val;\n    }\n    friend std::ostream &operator<<(std::ostream\
    \ &os, const fibonacci_heap &hp) {\n        os << \"[(fibonacci_heap: sz=\" <<\
    \ hp.sz << \", top=\" << hp.ptop->val << \", #tree = \" << hp.roots.size() <<\
    \ \")\";\n        for (auto x : hp.roots) { os << *x << \", \"; }\n        os\
    \ << ']';\n        return os;\n    }\n};\n\n#include <utility>\n#include <vector>\n\
    template <typename Tp> struct heap {\n    using P = std::pair<Tp, int>;\n    fibonacci_heap<P>\
    \ _heap;\n    std::vector<typename fibonacci_heap<P>::Node *> vp;\n    std::vector<Tp>\
    \ result;\n    void initialize(int N, Tp initval) {\n        _heap.clear();\n\
    \        vp.resize(N);\n        result.assign(N, initval);\n        for (int i\
    \ = 0; i < N; i++) { vp[i] = _heap.push(std::make_pair(initval, i)); }\n    }\n\
    \    heap(int N, Tp initval) { initialize(N, initval); }\n    bool chmin(int i,\
    \ Tp val) {\n        if (val < result[i]) {\n            result[i] = val;\n  \
    \          if (vp[i] == nullptr) {\n                vp[i] = _heap.push(std::make_pair(result[i],\
    \ i));\n            } else {\n                _heap.decrease(vp[i], std::make_pair(result[i],\
    \ i));\n            }\n            return true;\n        }\n        return false;\n\
    \    }\n    Tp operator[](int i) const { return result.at(i); }\n    P top() {\
    \ return _heap.top(); }\n    P pop() {\n        P ret = _heap.top();\n       \
    \ _heap.pop();\n        return ret;\n    }\n    int size() { return _heap.size();\
    \ }\n    bool empty() { return _heap.empty(); }\n};\n#line 5 \"data_structure/test/fibonacci_heap_shortestpath.test.cpp\"\
    \n#include <stack>\n#line 8 \"data_structure/test/fibonacci_heap_shortestpath.test.cpp\"\
    \n\nint main() {\n    std::cin.tie(nullptr), std::ios::sync_with_stdio(false);\n\
    \n    int N, M, s, t;\n    std::cin >> N >> M >> s >> t;\n    using P = std::pair<long\
    \ long, int>;\n    fibonacci_heap<P> heap;\n\n    constexpr long long INF = 1e18;\n\
    \    std::vector<long long> D(N, INF);\n    std::vector<int> prev(N, -1);\n  \
    \  D[s] = 0;\n    std::vector<fibonacci_heap<P>::Node *> handler(N);\n    for\
    \ (int i = 0; i < N; i++) { handler[i] = heap.push(std::make_pair(D[i], i)); }\n\
    \n    std::vector<std::vector<std::pair<int, int>>> to(N);\n    while (M--) {\n\
    \        int a, b, c;\n        std::cin >> a >> b >> c;\n        to[a].emplace_back(b,\
    \ c);\n    }\n\n    while (heap.size()) {\n        int now = heap.top().second;\n\
    \        if (now == t) break;\n        heap.pop();\n        for (const auto &nxtlen\
    \ : to[now]) {\n            const auto nxt = nxtlen.first, len = nxtlen.second;\n\
    \            if (D[nxt] > D[now] + len) {\n                D[nxt] = D[now] + len,\
    \ prev[nxt] = now;\n                heap.decrease(handler[nxt], std::make_pair(D[nxt],\
    \ nxt));\n            }\n        }\n    }\n\n    if (D[t] == INF) {\n        std::cout\
    \ << -1 << '\\n';\n        return 0;\n    }\n\n    std::stack<int> path;\n   \
    \ int now = t;\n    while (now != s) { path.push(now), now = prev[now], path.push(now);\
    \ }\n    std::cout << D[t] << ' ' << path.size() / 2 << '\\n';\n    while (path.size())\
    \ {\n        std::cout << path.top() << ' ';\n        path.pop();\n        std::cout\
    \ << path.top() << '\\n';\n        path.pop();\n    }\n}\n"
  code: "#define PROBLEM \"https://judge.yosupo.jp/problem/shortest_path\"\n\n#include\
    \ \"../fibonacci_heap.hpp\"\n#include <iostream>\n#include <stack>\n#include <utility>\n\
    #include <vector>\n\nint main() {\n    std::cin.tie(nullptr), std::ios::sync_with_stdio(false);\n\
    \n    int N, M, s, t;\n    std::cin >> N >> M >> s >> t;\n    using P = std::pair<long\
    \ long, int>;\n    fibonacci_heap<P> heap;\n\n    constexpr long long INF = 1e18;\n\
    \    std::vector<long long> D(N, INF);\n    std::vector<int> prev(N, -1);\n  \
    \  D[s] = 0;\n    std::vector<fibonacci_heap<P>::Node *> handler(N);\n    for\
    \ (int i = 0; i < N; i++) { handler[i] = heap.push(std::make_pair(D[i], i)); }\n\
    \n    std::vector<std::vector<std::pair<int, int>>> to(N);\n    while (M--) {\n\
    \        int a, b, c;\n        std::cin >> a >> b >> c;\n        to[a].emplace_back(b,\
    \ c);\n    }\n\n    while (heap.size()) {\n        int now = heap.top().second;\n\
    \        if (now == t) break;\n        heap.pop();\n        for (const auto &nxtlen\
    \ : to[now]) {\n            const auto nxt = nxtlen.first, len = nxtlen.second;\n\
    \            if (D[nxt] > D[now] + len) {\n                D[nxt] = D[now] + len,\
    \ prev[nxt] = now;\n                heap.decrease(handler[nxt], std::make_pair(D[nxt],\
    \ nxt));\n            }\n        }\n    }\n\n    if (D[t] == INF) {\n        std::cout\
    \ << -1 << '\\n';\n        return 0;\n    }\n\n    std::stack<int> path;\n   \
    \ int now = t;\n    while (now != s) { path.push(now), now = prev[now], path.push(now);\
    \ }\n    std::cout << D[t] << ' ' << path.size() / 2 << '\\n';\n    while (path.size())\
    \ {\n        std::cout << path.top() << ' ';\n        path.pop();\n        std::cout\
    \ << path.top() << '\\n';\n        path.pop();\n    }\n}\n"
  dependsOn:
  - data_structure/fibonacci_heap.hpp
  isVerificationFile: true
  path: data_structure/test/fibonacci_heap_shortestpath.test.cpp
  requiredBy: []
  timestamp: '2021-02-26 23:47:50+09:00'
  verificationStatus: TEST_ACCEPTED
  verifiedWith: []
documentation_of: data_structure/test/fibonacci_heap_shortestpath.test.cpp
layout: document
redirect_from:
- /verify/data_structure/test/fibonacci_heap_shortestpath.test.cpp
- /verify/data_structure/test/fibonacci_heap_shortestpath.test.cpp.html
title: data_structure/test/fibonacci_heap_shortestpath.test.cpp
---