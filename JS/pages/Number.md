- Includes Integer and Floating-Point Numbers
  There are some special values also stored in Numeric type,
  ``Infinity``, ``-Infinity``, ``NaN``
- For  ex.:
  ```js
  let x = 2/0; // stores Infinity in x
  x= Infinity //same
  x= "yo"/3; //error, stores NaN in x
  x= x*3; //still NaN
  ```
  NaN is a result of any computational error and any mathematical operation with NaN results back in NaN. This is also to say, ``maths is safe`` in JS as it would never crash the script, just store NaN in the worst case.
- Integers: $$\pm {(2^{53} - 3)}$$
  BigInt: Implementation-Defined Range, For chrome it is >1 billion bits
- Ints don't wrap around in JS, this means
  ```js
  console.log(9007199254740991 + 1); // 9007199254740992
  console.log(9007199254740991 + 2); // 9007199254740992
  ```
  BigInts have a much larger limit, however, using them is not safe as there may be precision errors the more bits we use.
  To create a BigInt we append literal ``n`` to a Number.
  For ex.:
  ```js
  let x = 234n; //BigInt
  ```
-
-