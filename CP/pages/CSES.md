- 1731:
  Uses [[Trie]] and ideas from [[Longest Common Subsequence]] Algorithm.
  
  To solve this problem, we can first build a trie from the given strings. Then for the main string ``s``, we use [[Dynamic Programming]] to look for all possible pairs. To do so, we start from the end of the main string ``L``, and pass the index ``x``, for each value of ``x``, we go from ``x`` to ``L`` in ``i``, and look if the given string ``s[i..L]`` exists in the Trie. [[TODO]]
  
  In C++,
  ```cpp
  
  int testCases{1};
  string s{};
  int sz{};
  int n{};
  int A{};    // alphabet set/lexicon size
  vvi trie{}; // we can use std::array too but the call-stack size is limited,
              // rather use heap
  vb eos{};   // end-of-string
  int counter{};
  vi dpTable{};
  
  void setup() {
    getline(cin, s);
    INPUT(n);
    A = 26;
    sz = cast(s.size());
    trie = vvi(cBN, vi(A, 0));
    eos = vb(cBN, false);
    counter = 0;
    dpTable = vi(sz + 1, 0);
    string arg{};
    cin.ignore(cN,'\n');
    while (n-- > 0) {
      getline(cin, arg);
      trie_insert(arg);
    }
  }
   
  void compute() {
    dpTable[sz] = 1;
    for (int i{sz - 1}; i >= 0; --i) {
      dpTable[i] = search(i);
    }
  }
  void trie_insert(string_view s) {
    int size_s{cast(s.size())};
    int next{};
    for (int i{}; i < size_s; ++i) {
      int &elem{trie[next][cast(s[i]%A)]};
      if (!elem)
        elem = ++counter;
   
      next = elem;
    }
    eos[next] = true;
  }
  int search(int x) {
    int next{};
    int ans{};
    for (int i{x}; i < sz; ++i) {
      int &elem{trie[next][cast(s[i]%A)]};
      if (!elem)
        return ans;
      next = elem;
      if (eos[next]) {
        ans += dpTable[i + 1];
        ans %= mod10_e9_7;
      }
    }
    return ans;
  }
   
  void output() { cout << dpTable[0] << "\n"; }
  
  ```
  
  Using [[Default C++ Template]]
-
-