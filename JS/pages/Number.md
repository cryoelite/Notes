- [[Object]] wrapper for primitive type ``number``
- Regular Numbers are stored in 64-bit format so there are always 64 bits to store any number, 52 bits are for the actual value of the number, 11 bits for the decimal position and 1 bit for the sign.
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
  [[BigInt]]s have a much larger limit, however, using them is not safe as there may be precision errors the more bits we use.
  To create a BigInt we append literal ``n`` to a Number.
  For ex.:
  ```js
  let x = 234n; //BigInt
  ```
- Numbers can use ``e`` which is the same as in [[C++]].
  ```js
  let x = 1e3; //1 * 1000
  ```
- Numbers can use ``_`` to break up larger values.
- By default, numbers are in decimal. To write other systems we prefix the number:
  Octal: ``0o``
  Hex: ``0x``
  Binary: ``0b``
- ``.toString(<optional base>)`` converts a number to int and returns a string.
  The method accepts a number between 2 to 36 and converts the number to that base and returns the value in string.
  For ex.:
  ```js
  let x = 255;
  x.toString(16); //returns "ff"
  ```
- The ``Math`` [[Object]] is an inbuilt JS Object that contains many important Math functions like ``Math.trunc(<num>)``. 
  Reference: [Mozilla Doc](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)
- ``.toFixed(<num>)`` rounds a number to given number of digits.
- ``Infinity`` is greater than any other Number in [[Comparison]] to a Number, similarly ``-Infinity`` is smaller.
- IsNaN(<value>) converts given arg to Number and returns true if it is equal to NaN. We can't compare [[NaN]] to anything, not even itself. This is why we need to use this special [[Function]] to check if a Number is NaN.
- isFinite(<value>) returns false if the value is ``Infinity``/``-Infinity``/``NaN``.
- Both isNaN and isFinite also exist in ``Number`` [[Object]], these are more stricter variants that only accept number type as argument.
- -0 and 0 can be both stored as Numbers in JS.
- ``parseInt(<value>,<optional base>)`` and ``parseFloat(<value>,<optional base>)`` read the value for a number until a character that isn't a Number is found, then returns the result. This is better than ``+"num"`` or ``Number()`` which requires value to be strictly a number or returns NaN. 
  
  The optional base called ``radix``  specifies which base the value is of. Default value is 10.
-