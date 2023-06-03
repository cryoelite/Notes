- Compares values using the comparison [[Operator]]
- There's Normal Equality, using == and !=. And [[Strict Equality]] using ``===`` and ``!==``.
- There's another oddity with normal equality comparison, the null and undefined comparison.
  null and undefined always equal each other and theirselves in normal equality comparison.
  For ex.:
  ```js
  let x= null==undefined; //true and they only equal each other and theirselves.
  ```
- For other operations, [[null]] and [[undefined]] are converted to [[Number]] where [[null]] becomes 0 and [[undefined]] becomes [[NaN]]. 
  Note, normal equality comparison doesn't convert [[null]] or [[undefined]] and only returns true when compared against theirselves or each other, but other comparison operators do.
  This leads to following behavior:
  ```js
  let x= null > 0; //false, converts null to 0
  x = null==0; //false, compares null to 0
  x= null >= 0; //true, converts null to 0
  ```
  On the contrary, undefined always returns false when not normal equality compared against itself or null, because it gets converted to [[NaN]].
-