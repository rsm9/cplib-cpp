---
data:
  _extendedDependsOn:
  - icon: ':heavy_check_mark:'
    path: flow/mincostflow_bellmanford.hpp
    title: flow/mincostflow_bellmanford.hpp
  _extendedRequiredBy: []
  _extendedVerifiedWith: []
  _pathExtension: cpp
  _verificationStatusIcon: ':heavy_check_mark:'
  attributes:
    '*NOT_SPECIAL_COMMENTS*': ''
    PROBLEM: https://judge.yosupo.jp/problem/assignment
    links:
    - https://judge.yosupo.jp/problem/assignment
  bundledCode: "#line 2 \"flow/mincostflow_bellmanford.hpp\"\n#include <cassert>\n\
    #include <iostream>\n#include <limits>\n#include <queue>\n#include <vector>\n\n\
    // CUT begin\n/*\nMinCostFlow: Minimum-cost flow problem solver WITH NO NEGATIVE\
    \ CYCLE (just negative cost edge is allowed)\nVerified by SRM 770 Div1 Medium\
    \ <https://community.topcoder.com/stat?c=problem_statement&pm=15702>\n*/\ntemplate\
    \ <typename CAP = long long, typename COST = long long>\nstruct MinCostFlow\n\
    {\n    const COST INF_COST = std::numeric_limits<COST>::max() / 2;\n    struct\
    \ edge {\n        int to, rev;\n        CAP cap;\n        COST cost;\n       \
    \ friend std::ostream &operator<<(std::ostream &os, const edge &e) {\n       \
    \     os << '(' << e.to << ',' << e.rev << ',' << e.cap << ',' << e.cost << ')';\n\
    \            return os;\n        }\n    };\n    int V;\n    std::vector<std::vector<edge>>\
    \ g;\n    std::vector<COST> dist;\n    std::vector<int> prevv, preve;\n    std::vector<COST>\
    \ dual;  // dual[V]: potential\n    std::vector<std::pair<int, int>> pos;\n\n\
    \    bool _calc_distance_bellman_ford(int s) {  // O(VE), able to detect negative\
    \ cycle\n        dist.assign(V, INF_COST);\n        dist[s] = 0;\n        bool\
    \ upd = true;\n        int cnt = 0;\n        while (upd) {\n            upd =\
    \ false;\n            cnt++;\n            if (cnt > V) return false;  // Negative\
    \ cycle existence\n            for (int v = 0; v < V; v++) if (dist[v] != INF_COST)\
    \ {\n                for (int i = 0; i < (int)g[v].size(); i++) {\n          \
    \          edge &e = g[v][i];\n                    COST c = dist[v] + e.cost +\
    \ dual[v] - dual[e.to];\n                    if (e.cap > 0 and dist[e.to] > c)\
    \ {\n                        dist[e.to] = c, prevv[e.to] = v, preve[e.to] = i;\n\
    \                        upd = true;\n                    }\n                }\n\
    \            }\n        }\n        return true;\n    }\n\n    bool _calc_distance_dijkstra(int\
    \ s) {  // O(ElogV)\n        dist.assign(V, INF_COST);\n        dist[s] = 0;\n\
    \        using P = std::pair<COST, int>;\n        std::priority_queue<P, std::vector<P>,\
    \ std::greater<P>> q;\n        q.emplace(0, s);\n        while (!q.empty()) {\n\
    \            P p = q.top();\n            q.pop();\n            int v = p.second;\n\
    \            if (dist[v] < p.first) continue;\n            for (int i = 0; i <\
    \ (int)g[v].size(); i++) {\n                edge &e = g[v][i];\n             \
    \   COST c = dist[v] + e.cost + dual[v] - dual[e.to];\n                if (e.cap\
    \ > 0 and dist[e.to] > c) {\n                    dist[e.to] = c, prevv[e.to] =\
    \ v, preve[e.to] = i;\n                    q.emplace(dist[e.to], e.to);\n    \
    \            }\n            }\n        }\n        return true;\n    }\n\n    MinCostFlow(int\
    \ V=0) : V(V), g(V) {}\n\n    void add_edge(int from, int to, CAP cap, COST cost)\
    \ {\n        assert(0 <= from and from < V);\n        assert(0 <= to and to <\
    \ V);\n        pos.emplace_back(from, g[from].size());\n        g[from].emplace_back(edge{to,\
    \ (int)g[to].size() + (from == to), cap, cost});\n        g[to].emplace_back(edge{from,\
    \ (int)g[from].size() - 1, (CAP)0, -cost});\n    }\n\n    std::pair<CAP, COST>\
    \ flow(int s, int t, const CAP &f) {\n        /*\n        Flush amount of `f`\
    \ from `s` to `t` using the Dijkstra's algorithm\n        works for graph with\
    \ no negative cycles (negative cost edges are allowed)\n        retval: (flow,\
    \ cost)\n        */\n        COST ret = 0;\n        dual.assign(V, 0);\n     \
    \   prevv.assign(V, -1);\n        preve.assign(V, -1);\n        CAP frem = f;\n\
    \        while (frem > 0) {\n            _calc_distance_dijkstra(s);\n       \
    \     if (dist[t] == INF_COST) break;\n            for (int v = 0; v < V; v++)\
    \ dual[v] = std::min(dual[v] + dist[v], INF_COST);\n            CAP d = frem;\n\
    \            for (int v = t; v != s; v = prevv[v]) {\n                d = std::min(d,\
    \ g[prevv[v]][preve[v]].cap);\n            }\n            frem -= d;\n       \
    \     ret += d * dual[t];\n            for (int v = t; v != s; v = prevv[v]) {\n\
    \                edge &e = g[prevv[v]][preve[v]];\n                e.cap -= d;\n\
    \                g[v][e.rev].cap += d;\n            }\n        }\n        return\
    \ std::make_pair(f - frem, ret);\n    }\n\n    friend std::ostream &operator<<(std::ostream\
    \ &os, const MinCostFlow &mcf) {\n        os << \"[MinCostFlow]V=\" << mcf.V <<\
    \ \":\";\n        for (int i = 0; i < (int)mcf.g.size(); i++) for (auto &e : mcf.g[i])\
    \ {\n            os << \"\\n\" << i << \"->\" << e.to << \": cap=\" << e.cap <<\
    \ \", cost=\" << e.cost;\n        }\n        return os;\n    }\n};\n#line 2 \"\
    flow/test/assignment_problem.test.cpp\"\n#define PROBLEM \"https://judge.yosupo.jp/problem/assignment\"\
    \n#include <algorithm>\n#line 5 \"flow/test/assignment_problem.test.cpp\"\n\n\
    template<typename TC>\nstd::pair<TC, std::vector<int>> AssignmentProblem(std::vector<std::vector<TC>>\
    \ cost)\n{\n    int N = cost.size();\n    MinCostFlow<long long, long long> mcf(N\
    \ * 2 + 2);\n    int S = N * 2, T = N * 2 + 1;\n    TC bias_total_cost = 0;\n\
    \    for (int i = 0; i < N; i++) {\n        TC lo = *min_element(cost[i].begin(),\
    \ cost[i].end());\n        bias_total_cost += lo;\n        mcf.add_edge(S, i,\
    \ 1, 0);\n        mcf.add_edge(N + i, T, 1, 0);\n        for (int j = 0; j < N;\
    \ j++)\n        {\n            mcf.add_edge(i, N + j, 1, cost[i][j] - lo);\n \
    \       }\n    }\n    auto total_cost = mcf.flow(S, T, N).second + bias_total_cost;\n\
    \    std::vector<int> ret;\n\n    for (int i = 0; i < N; i++)\n    {\n       \
    \ for (const auto &g : mcf.g[i])\n        {\n            if (g.to != S and !g.cap)\n\
    \            {\n                ret.emplace_back(g.to - N);\n                break;\n\
    \            }\n        }\n    }\n    return std::make_pair(total_cost, ret);\n\
    }\n\n\nint main()\n{\n    int N;\n    std::cin >> N;\n    std::vector<std::vector<long\
    \ long>> A(N, std::vector<long long>(N));\n    for (auto &vec : A)\n    {\n  \
    \      for (auto &x : vec)\n        {\n            std::cin >> x;\n        }\n\
    \    }\n    auto ret = AssignmentProblem(A);\n    std::cout << ret.first << '\\\
    n';\n    for (auto x : ret.second) std::cout << x << ' ';\n    std::cout << '\\\
    n';\n}\n"
  code: "#include \"flow/mincostflow_bellmanford.hpp\"\n#define PROBLEM \"https://judge.yosupo.jp/problem/assignment\"\
    \n#include <algorithm>\n#include <iostream>\n\ntemplate<typename TC>\nstd::pair<TC,\
    \ std::vector<int>> AssignmentProblem(std::vector<std::vector<TC>> cost)\n{\n\
    \    int N = cost.size();\n    MinCostFlow<long long, long long> mcf(N * 2 + 2);\n\
    \    int S = N * 2, T = N * 2 + 1;\n    TC bias_total_cost = 0;\n    for (int\
    \ i = 0; i < N; i++) {\n        TC lo = *min_element(cost[i].begin(), cost[i].end());\n\
    \        bias_total_cost += lo;\n        mcf.add_edge(S, i, 1, 0);\n        mcf.add_edge(N\
    \ + i, T, 1, 0);\n        for (int j = 0; j < N; j++)\n        {\n           \
    \ mcf.add_edge(i, N + j, 1, cost[i][j] - lo);\n        }\n    }\n    auto total_cost\
    \ = mcf.flow(S, T, N).second + bias_total_cost;\n    std::vector<int> ret;\n\n\
    \    for (int i = 0; i < N; i++)\n    {\n        for (const auto &g : mcf.g[i])\n\
    \        {\n            if (g.to != S and !g.cap)\n            {\n           \
    \     ret.emplace_back(g.to - N);\n                break;\n            }\n   \
    \     }\n    }\n    return std::make_pair(total_cost, ret);\n}\n\n\nint main()\n\
    {\n    int N;\n    std::cin >> N;\n    std::vector<std::vector<long long>> A(N,\
    \ std::vector<long long>(N));\n    for (auto &vec : A)\n    {\n        for (auto\
    \ &x : vec)\n        {\n            std::cin >> x;\n        }\n    }\n    auto\
    \ ret = AssignmentProblem(A);\n    std::cout << ret.first << '\\n';\n    for (auto\
    \ x : ret.second) std::cout << x << ' ';\n    std::cout << '\\n';\n}\n"
  dependsOn:
  - flow/mincostflow_bellmanford.hpp
  isVerificationFile: true
  path: flow/test/assignment_problem.test.cpp
  requiredBy: []
  timestamp: '2020-11-15 19:54:08+09:00'
  verificationStatus: TEST_ACCEPTED
  verifiedWith: []
documentation_of: flow/test/assignment_problem.test.cpp
layout: document
redirect_from:
- /verify/flow/test/assignment_problem.test.cpp
- /verify/flow/test/assignment_problem.test.cpp.html
title: flow/test/assignment_problem.test.cpp
---