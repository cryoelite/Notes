- ``loop``, ``while`` and ``for``. ``break`` and ``continue`` are also expressions, and can be
  used with loops for some neat operations.
- ``break``
  Breaks the loop. Unlike other langs, break in Rust can also return values.
- ``loop``
  Infinite loop.
  For ex.:
  ```rust
      let mut x= 1;
      let z = loop {
          if x==20 {
             break 25 //semicolon not required, but can be put.
          }
          x+=1;
      }; //puts 25 in z
  ```
  
  * loop label: Using a custom keyword we can break/continue a specific loop. 
  For ex.
  ```rust
      let z = 'xyz_loop: loop {
          if x==20 {
             'two: loop {
              if x==25 {
                  break 'two;
              }
              x+=1;
             }
             break 'xyz_loop x;
          }
          x+=1;
      };
  ```
- ``while``
  Works as other langs.
  ``while <condition> {
  …
  }``
  
  * while let: Using [[Pattern Matching]], while let is another form of while that also initializes a variable.
  For ex.:
  ```rust
      let mut stack = Vec::new();
   
      stack.push(1);
      stack.push(2);
      stack.push(3);
   
      while let Some(top) = stack.pop() {
          println!("{}", top);
      }
  
  ```
- ``for``
  In Rust, only ``for..in`` type of Loop exists. So it can only loop over [[Collection]]s or more specifically [[Iterator]]s.
  For ex.:
  ```rust
  let arr = [1,2,5,3]
  for element in arr { //copies/moves each element to element and then passes it to the body
  	//yuh
  }
  ```
  
  * We can create an [[Iterable]] and assign it directly to a loop with [[Range]].
  For ex.:
  ```rust
  for elem in (1..4).rev() { \\ 1..4 generates a Range with 3 elements 1, 2, 3 and rev() reverses the range. 
   \\aye
  }
  ```
  * for with index: If we need the index too, we can use [[Destructuring]] and the ``.iter()`` [[Function]] already implemented for all [[Iterable]]s.
  For ex.:
  ```rust
  for (index, elem) in arr.iter().enumerate() {
  //
  } //works
  ```
  ``.iter()`` is already defined for [T;KSize] [[Data Type]]s and enumerate() on it returns a tuple with index and value.
-
-
-