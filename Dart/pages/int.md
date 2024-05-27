- superclass: [[num]]
- 64-bit integers, on native platforms, from $$-2^{63}$$ to $$2^{63}-1$$. On browsers ints are represented as [[Javascript]] numbers, so range is  $$-2^{53}$$ to $$2^{53}-1$$
- `nums` that don't have a decimal point.
  Define them with literals like so
  ```dart
  var x =1;
  var y = 0xDEADBEEF;
  ```
- Has the default bitwise [[Operator]]s.
  FE:
  ```dart
  assert((3 & 4) == 0); // 0011 & 0100 == 0000
  ```
- Can't hold non-integer values, or [[NaN]].
-
-
-