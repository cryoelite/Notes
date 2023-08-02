- ``Result<T,E>`` 
  An [[Enum]] which is defined in the [[Prelude]] like so
  
  ```rust
  enum Result<T, E> {
      Ok(T),
      Err(E),
  }
  ```
  It works much like the [[Option Type]] but it is meant to be used with [[Error]]s to represent Recoverable Errors.
  
  For ex.:
  ```rust
  use std::fs::File;
  
  fn main() {
      let greeting_file_result = File::open("hello.txt");
  
      let greeting_file = match greeting_file_result {
          Ok(file) => file,
          Err(error) => panic!("Problem opening the file: {:?}", error),
      };
  }
  ```
- Just like the [[Option Type]], the keyword ``Ok`` and ``Err`` are available as-is as well.
-
-