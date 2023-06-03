- This operator takes 2 [[Operand]]s and checks if the first operand is [[null]] or [[undefined]], if it is either then the second Operand is returned.
  For ex.:
  ```js
  let x= null ?? 2; //returns 2
  ```
- It's kind of like the Logical Operator OR, but where OR returns the first truthy value, i.e., value that is not false (like 0), this [[Operator]] returns the first defined value (like 0).
  For ex.:
  ```js
  let x= 0 || 1; //returns 1 
  x= 0 ?? 1; //returns 0
  ```
-