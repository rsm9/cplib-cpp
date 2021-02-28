---
data:
  _extendedDependsOn:
  - icon: ':heavy_check_mark:'
    path: geometry/geometry2d.hpp
    title: geometry/geometry2d.hpp
  _extendedRequiredBy: []
  _extendedVerifiedWith: []
  _isVerificationFailed: false
  _pathExtension: cpp
  _verificationStatusIcon: ':heavy_check_mark:'
  attributes:
    '*NOT_SPECIAL_COMMENTS*': ''
    PROBLEM: http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=CGL_4_A
    links:
    - http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=CGL_4_A
  bundledCode: "#line 2 \"geometry/geometry2d.hpp\"\n#include <algorithm>\n#include\
    \ <cassert>\n#include <cmath>\n#include <iostream>\n#include <utility>\n#include\
    \ <vector>\n\n// CUT begin\ntemplate <typename T_P> struct P {\n    static T_P\
    \ EPS;\n    static void set_eps(T_P e) { EPS = e; }\n    T_P x, y;\n    P() :\
    \ x(0), y(0) {}\n    P(T_P x, T_P y) : x(x), y(y) {}\n    P(std::pair<T_P, T_P>\
    \ p) : x(p.first), y(p.second) {}\n    P operator+(const P &p) const noexcept\
    \ { return P(x + p.x, y + p.y); }\n    P operator-(const P &p) const noexcept\
    \ { return P(x - p.x, y - p.y); }\n    P operator*(const P &p) const noexcept\
    \ { return P(x * p.x - y * p.y, x * p.y + y * p.x); }\n    P operator*(T_P d)\
    \ const noexcept { return P(x * d, y * d); }\n    P operator/(T_P d) const noexcept\
    \ { return P(x / d, y / d); }\n    P inv() const { return conj() / norm2(); }\n\
    \    P operator/(const P &p) const { return (*this) * p.inv(); }\n    bool operator<(const\
    \ P &r) const noexcept { return x != r.x ? x < r.x : y < r.y; }\n    bool operator==(const\
    \ P &r) const noexcept { return x == r.x and y == r.y; }\n    bool operator!=(const\
    \ P &r) const noexcept { return !((*this) == r); }\n    T_P dot(P p) const noexcept\
    \ { return x * p.x + y * p.y; }\n    T_P det(P p) const noexcept { return x *\
    \ p.y - y * p.x; }\n    T_P norm() const noexcept { return std::sqrt(x * x + y\
    \ * y); }\n    T_P norm2() const noexcept { return x * x + y * y; }\n    T_P arg()\
    \ const noexcept { return std::atan2(y, x); }\n    // rotate point/vector by rad\n\
    \    P rotate(T_P rad) noexcept { return P(x * std::cos(rad) - y * std::sin(rad),\
    \ x * std::sin(rad) + y * std::cos(rad)); }\n    P normalized() const { return\
    \ (*this) / this->norm(); }\n    P conj() const noexcept { return P(x, -y); }\n\
    \    friend std::istream &operator>>(std::istream &is, P &p) {\n        T_P x,\
    \ y;\n        is >> x >> y;\n        p = P(x, y);\n        return is;\n    }\n\
    \    friend std::ostream &operator<<(std::ostream &os, const P &p) {\n       \
    \ os << '(' << p.x << ',' << p.y << ')';\n        return os;\n    }\n};\ntemplate\
    \ <> double P<double>::EPS = 1e-9;\ntemplate <> long double P<long double>::EPS\
    \ = 1e-12;\n\ntemplate <typename T_P>\nint ccw(const P<T_P> &a, const P<T_P> &b,\
    \ const P<T_P> &c) // a->b->c\u306E\u66F2\u304C\u308A\u65B9\n{\n    P<T_P> v1\
    \ = b - a;\n    P<T_P> v2 = c - a;\n    if (v1.det(v2) > P<T_P>::EPS) return 1;\
    \   // \u5DE6\u6298\n    if (v1.det(v2) < -P<T_P>::EPS) return -1; // \u53F3\u6298\
    \n    if (v1.dot(v2) < -P<T_P>::EPS) return 2;  // c-a-b\n    if (v1.norm() <\
    \ v2.norm()) return -2;     // a-b-c\n    return 0;                          \
    \       // a-c-b\n}\n\n// Convex hull \uFF08\u51F8\u5305\uFF09\n// return: IDs\
    \ of vertices used for convex hull, counterclockwise\n// include_boundary: If\
    \ true, interior angle pi is allowed\ntemplate <typename T_P> std::vector<int>\
    \ convex_hull(const std::vector<P<T_P>> &ps, bool include_boundary = false) {\n\
    \    int n = ps.size();\n    if (n <= 1) return std::vector<int>(n, 0);\n    std::vector<std::pair<P<T_P>,\
    \ int>> points(n);\n    for (size_t i = 0; i < ps.size(); i++) points[i] = std::make_pair(ps[i],\
    \ i);\n    std::sort(points.begin(), points.end());\n    int k = 0;\n    std::vector<std::pair<P<T_P>,\
    \ int>> qs(2 * n);\n    auto ccw_check = [&](int c) {\n        if (include_boundary)\n\
    \            return c == -1;\n        else\n            return c <= 0;\n    };\n\
    \    for (int i = 0; i < n; i++) {\n        while (k > 1 and ccw_check(ccw(qs[k\
    \ - 2].first, qs[k - 1].first, points[i].first))) k--;\n        qs[k++] = points[i];\n\
    \    }\n    for (int i = n - 2, t = k; i >= 0; i--) {\n        while (k > t and\
    \ ccw_check(ccw(qs[k - 2].first, qs[k - 1].first, points[i].first))) k--;\n  \
    \      qs[k++] = points[i];\n    }\n    std::vector<int> ret(k - 1);\n    for\
    \ (int i = 0; i < k - 1; i++) ret[i] = qs[i].second;\n    return ret;\n}\n\n//\
    \ Solve r1 + t1 * v1 == r2 + t2 * v2\ntemplate <typename T_P> P<T_P> lines_crosspoint(P<T_P>\
    \ r1, P<T_P> v1, P<T_P> r2, P<T_P> v2) {\n    assert(v2.det(v1) != 0);\n    return\
    \ r1 + v1 * (v2.det(r2 - r1) / v2.det(v1));\n}\n\n// Convex cut\n// Cut the convex\
    \ polygon g by line p1->p2 and return the leftward one\ntemplate <typename T_P>\
    \ std::vector<P<T_P>> convex_cut(const std::vector<P<T_P>> &g, P<T_P> p1, P<T_P>\
    \ p2) {\n    assert(p1 != p2);\n    std::vector<P<T_P>> ret;\n    for (int i =\
    \ 0; i < (int)g.size(); i++) {\n        const P<T_P> &now = g[i], &nxt = g[(i\
    \ + 1) % g.size()];\n        if (ccw(p1, p2, now) != -1) ret.push_back(now);\n\
    \        if ((ccw(p1, p2, now) == -1) xor (ccw(p1, p2, nxt) == -1)) { ret.push_back(lines_crosspoint(now,\
    \ nxt - now, p1, p2 - p1)); }\n    }\n    return ret;\n}\n\n// Circumcenter \uFF08\
    \u4E09\u89D2\u5F62\u306E\u5916\u5FC3\uFF09\n// - raise exception for collinear\
    \ points\ntemplate <typename T_P> P<T_P> circumcenter(const P<T_P> &z1, const\
    \ P<T_P> &z2, const P<T_P> &z3) {\n    assert(abs(ccw(z1, z2, z3)) % 2 == 1);\n\
    \    P<T_P> a = z2 - z1, b = z3 - z1;\n    return z1 + a * b * (a - b).conj()\
    \ / (b * a.conj() - a * b.conj());\n}\n\n// 2\u5186\u306E\u4EA4\u70B9 (ABC157F)\n\
    template <typename T_P> std::vector<P<T_P>> IntersectTwoCircles(const P<T_P> &Ca,\
    \ double Ra, const P<T_P> &Cb, double Rb) {\n    double d = (Ca - Cb).norm();\n\
    \    if (Ra + Rb < d) return {};\n    double rc = (d * d + Ra * Ra - Rb * Rb)\
    \ / (2 * d);\n    double rs = sqrt(Ra * Ra - rc * rc);\n    P<T_P> diff = (Cb\
    \ - Ca) / d;\n    return {Ca + diff * P<T_P>(rc, rs), Ca + diff * P<T_P>(rc, -rs)};\n\
    }\n\n// Solve |x0 + vt| = R (SRM 543 Div.1 1000)\ntemplate <typename T_P> std::vector<T_P>\
    \ IntersectCircleLine(const P<T_P> &x0, const P<T_P> &v, T_P R) {\n    T_P b =\
    \ x0.dot(v) / v.norm2();\n    T_P c = (x0.norm2() - R * R) / v.norm2();\n    if\
    \ (b * b - c < 0) return {};\n    T_P ret1;\n    if (b > 0)\n        ret1 = -b\
    \ - sqrt(b * b - c);\n    else\n        ret1 = -b + sqrt(b * b - c);\n    T_P\
    \ ret2 = c / ret1;\n    std::vector<T_P> ret{ret1, ret2};\n    std::sort(ret.begin(),\
    \ ret.end());\n    return ret;\n}\n\n// Distance between point p <-> line ab\n\
    template <typename T_P> T_P DistancePointLine(const P<T_P> &p, const P<T_P> &a,\
    \ const P<T_P> &b) {\n    assert(a != b);\n    return std::abs((b - a).det(p -\
    \ a)) / (b - a).norm();\n}\n\n// Distance between point p <-> line segment ab\n\
    template <typename T_P> T_P DistancePointSegment(const P<T_P> &p, const P<T_P>\
    \ &a, const P<T_P> &b) {\n    if (a == b)\n        return (p - a).norm();\n  \
    \  else if ((p - a).dot(b - a) <= 0)\n        return (p - a).norm();\n    else\
    \ if ((p - b).dot(a - b) <= 0)\n        return (p - b).norm();\n    else\n   \
    \     return DistancePointLine(p, a, b);\n}\n\n// Area of polygon (might be negative)\n\
    template <typename T_P> T_P signed_area_of_polygon(const std::vector<P<T_P>> &poly)\
    \ {\n    T_P area = 0;\n    for (size_t i = 0; i < poly.size(); i++) { area +=\
    \ poly[i].det(poly[(i + 1) % poly.size()]); }\n    return area * 0.5;\n}\n#line\
    \ 2 \"geometry/test/convex_hull.test.cpp\"\n#define PROBLEM \"http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=CGL_4_A\"\
    \n#line 4 \"geometry/test/convex_hull.test.cpp\"\n#include <cstdio>\n#line 6 \"\
    geometry/test/convex_hull.test.cpp\"\n\nint main() {\n    int N;\n    std::cin\
    \ >> N;\n    std::vector<P<double>> P(N);\n    P[0].set_eps(1e-9);\n\n    for\
    \ (auto &p : P) std::cin >> p;\n    std::vector<std::pair<int, int>> ps;\n   \
    \ for (auto idx : convex_hull(P, true)) ps.emplace_back(std::llround(P[idx].y),\
    \ std::llround(P[idx].x));\n    int init = std::min_element(ps.begin(), ps.end())\
    \ - ps.begin();\n\n    printf(\"%lu\\n\", ps.size());\n    for (size_t i = 0;\
    \ i < ps.size(); i++) { printf(\"%d %d\\n\", ps[(i + init) % ps.size()].second,\
    \ ps[(i + init) % ps.size()].first); }\n}\n"
  code: "#include \"geometry/geometry2d.hpp\"\n#define PROBLEM \"http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=CGL_4_A\"\
    \n#include <cmath>\n#include <cstdio>\n#include <iostream>\n\nint main() {\n \
    \   int N;\n    std::cin >> N;\n    std::vector<P<double>> P(N);\n    P[0].set_eps(1e-9);\n\
    \n    for (auto &p : P) std::cin >> p;\n    std::vector<std::pair<int, int>> ps;\n\
    \    for (auto idx : convex_hull(P, true)) ps.emplace_back(std::llround(P[idx].y),\
    \ std::llround(P[idx].x));\n    int init = std::min_element(ps.begin(), ps.end())\
    \ - ps.begin();\n\n    printf(\"%lu\\n\", ps.size());\n    for (size_t i = 0;\
    \ i < ps.size(); i++) { printf(\"%d %d\\n\", ps[(i + init) % ps.size()].second,\
    \ ps[(i + init) % ps.size()].first); }\n}\n"
  dependsOn:
  - geometry/geometry2d.hpp
  isVerificationFile: true
  path: geometry/test/convex_hull.test.cpp
  requiredBy: []
  timestamp: '2020-11-18 20:25:12+09:00'
  verificationStatus: TEST_ACCEPTED
  verifiedWith: []
documentation_of: geometry/test/convex_hull.test.cpp
layout: document
redirect_from:
- /verify/geometry/test/convex_hull.test.cpp
- /verify/geometry/test/convex_hull.test.cpp.html
title: geometry/test/convex_hull.test.cpp
---