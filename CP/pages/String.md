- String s of n characters with alphabets({a,b,....z}, i.e., [[Set]] of lower-case Latin characters) is an array from s[0],s[1]....s[n-1].
- [[Substring]]
- [[Subsequence]]
- A ``prefix`` of a string s is any substring p formed from s that starts with the first character of s. For ex.:
  ```
  s= "BYTE"
  p= {"B", "BY", "BYT", "BYTE"} //p can be any of the substring from this set.
  ```
- A ``suffix`` of a string s is any substring p formed from s that ends with the last character of s. For ex.:
  ```
  s= "BYTE"
  p= {"E", "TE", "YTE", "BYTE"} //p can be any of the substring from this set.
  ```
- A ``border`` of a string s is any substring p formed from s that is both the prefix and the suffix of s. For ex.:
  ```
  s= "ABCDAB"
  p = "AB"; //AB can be both prefix and suffix for s, hence it is a border.
  ```
- A ``rotation`` of a string s is another string r that can be created by repeatedly shifting the first character to the end of s.
  For ex.:
  ```
  s= "STAR"
  r= "TARS"; //then r can be ARST then RSTA and so on.
  ```
- [[Trie]]
- [[Longest Common Subsequence]]
-
-