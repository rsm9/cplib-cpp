---
data:
  _extendedDependsOn:
  - icon: ':heavy_check_mark:'
    path: segmenttree/binary_indexed_tree.hpp
    title: segmenttree/binary_indexed_tree.hpp
  _extendedRequiredBy: []
  _extendedVerifiedWith: []
  _pathExtension: cpp
  _verificationStatusIcon: ':heavy_check_mark:'
  attributes:
    '*NOT_SPECIAL_COMMENTS*': ''
    PROBLEM: http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=DSL_2_B
    links:
    - http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=DSL_2_B
  bundledCode: "#line 2 \"segmenttree/binary_indexed_tree.hpp\"\n#include <iostream>\n\
    #include <vector>\n\n// CUT begin\n// 1-indexed BIT (i : [1, len])\ntemplate <typename\
    \ T>\nstruct BIT : std::vector<T>\n{\n    BIT(int len = 0) : std::vector<T>(len\
    \ + 1) {}\n    void reset() { fill(this->begin(), this->end(), 0); }\n    void\
    \ add(int pos, T v) {\n        while (pos > 0 and pos < (int)this->size()) (*this)[pos]\
    \ += v, pos += pos & -pos;\n    }\n    T sum(int pos) const { // (0, pos]\n  \
    \      T res = 0;\n        while (pos > 0) res += (*this)[pos], pos -= pos & -pos;\n\
    \        return res;\n    }\n    friend std::ostream &operator<<(std::ostream\
    \ &os, const BIT &bit) {\n        T prv = 0;\n        os << '[';\n        for\
    \ (int i = 1; i < (int)bit.size(); i++) {\n            T now = bit.sum(i);\n \
    \           os << now - prv << \",\";\n            prv = now;\n        }\n   \
    \     os << ']';\n        return os;\n    }\n};\n#line 2 \"segmenttree/test/binary_indexed_tree.test.cpp\"\
    \n#define PROBLEM \"http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=DSL_2_B\"\
    \n#include <cstdio>\n\nint main()\n{\n    int N, Q;\n    scanf(\"%d %d\", &N,\
    \ &Q);\n    BIT<int> bit(N);\n    while (Q--)\n    {\n        int c, x, y;\n \
    \       scanf(\"%d %d %d\", &c, &x, &y);\n        if (c) printf(\"%d\\n\", bit.sum(y)\
    \ - bit.sum(x - 1));\n        else bit.add(x, y);\n    }\n}\n"
  code: "#include \"segmenttree/binary_indexed_tree.hpp\"\n#define PROBLEM \"http://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=DSL_2_B\"\
    \n#include <cstdio>\n\nint main()\n{\n    int N, Q;\n    scanf(\"%d %d\", &N,\
    \ &Q);\n    BIT<int> bit(N);\n    while (Q--)\n    {\n        int c, x, y;\n \
    \       scanf(\"%d %d %d\", &c, &x, &y);\n        if (c) printf(\"%d\\n\", bit.sum(y)\
    \ - bit.sum(x - 1));\n        else bit.add(x, y);\n    }\n}\n"
  dependsOn:
  - segmenttree/binary_indexed_tree.hpp
  isVerificationFile: true
  path: segmenttree/test/binary_indexed_tree.test.cpp
  requiredBy: []
  timestamp: '2020-04-19 16:23:43+09:00'
  verificationStatus: TEST_ACCEPTED
  verifiedWith: []
documentation_of: segmenttree/test/binary_indexed_tree.test.cpp
layout: document
redirect_from:
- /verify/segmenttree/test/binary_indexed_tree.test.cpp
- /verify/segmenttree/test/binary_indexed_tree.test.cpp.html
title: segmenttree/test/binary_indexed_tree.test.cpp
---