- Both [[int]] and [[double]] are subtypes of this [[Data Type]], this is the type that implements basic operations like Arithmetic [[Operator]]s. It also defines other functions like `abs()`, `ceil()`,  `floor()`.
  
  Other numerical methods can be found in `dart:math` [[Library]]
- ``num``
  type can store both ints and doubles
  FE:
  ```dart
  num x = 1;
  x = 1.5; //works
  ```
- [[Conversion]]
- Literal numbers (numbers written manually) are compile-time constants. And [[Operator]]s that have compile-time constant operands also evaluate to compile-time constants.
- There's a special value, [[NaN]] that only ``num`` and ``double`` can hold. 
  FE:
  ```dart
  double x = 0/0;
  ```
  stores `NaN` in x. We can check if a `double` has an [[NaN]] with `<var>.isNaN` and it returns a [[bool]].
  
  For [[int]], this results in an ``IntegerDivisionByZero`` [[Exception]].
-
-
-