- superclass: [[num]]
- 64-bit double precision numbers as represented in IEEE 754.
- `nums` that have a decimal point.
  FE:
  ```dart
  var x =1.1;
  var y = 1.42e5;
  ```
  supports `e` which is `1.42 * 10^5`.
- ints are implicitly converted to doubles in a double [[Variable]].
- Can hold [[NaN]].
-