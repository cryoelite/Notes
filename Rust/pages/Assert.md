- An assert is simply a requirement, that the condition being checked must be true. If it isn't, this leads to a [[Panic]].
  
  We use asserts using the ``assert!`` [[Macro]].
  
  For ex.:
  ```rust
      fn exploration() {
          assert!(2 + 2 == 4);
      }
  ```
- Similarly there are other variants 
  * ``assert_eq!(<obj 1>,<obj 2>)``: If ``obj 1`` == ``obj 2``.
  * ``assert_ne!(<obj 1>,<obj 2>)``: If ``obj 1`` != ``obj 2``.
- Custom failure text
  After the required arguments to all ``assert`` [[Macro]]s , the [[String]] passed is given to ``format!`` [[Macro]] and it prints the given string.
-