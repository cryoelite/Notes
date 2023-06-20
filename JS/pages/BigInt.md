- [[Object]] wrapper for primitive type ``bigint``
  For ex.:
  ```js
  let x = 234n; //BigInt
  let y= BigInt(5); //also ok
  ```
- Unary [[Operator]] ``+`` is not supported on bigints.
- For comparisons, BigInts and [[Number]]s can be compared and work as expected. Same with [[Boolean]]s as a BigInt of ``0`` is false, rest are true.
- [[Polyfill]]ing BigInts is tricky as BigInts return BigInts on division/addition/etc. 
  So it is recommended to use [JSBI](https://github.com/GoogleChromeLabs/jsbi) if we need to polyfill as JSBI uses explicit methods for BigInt ops.