- Rust divides errors into 2 categories, *Recoverable* and *Non-Recoverable* Errors. 
  Simple errors like File not Found can be handled as their failing is expected so they are recoverable errors and are represented with the [[Result Type]], but errors such as accessing an invalid index in an [[Array]] is not expected so it is a type of non-recoverable error, these are triggered through the ``panic!`` [[Macro]].
- [[Panic]]
- The [[Result Type]] gets an Error [[Struct]] object when error .
- Different error types in Rust are represented through a different variant of the ``ErrorKind`` [[Enum]]
  So we can use ``<error obj>.kind()`` to get the kind and compare it.
  
  For ex.:
  ```rust
  use std::fs::File;
  use std::io::ErrorKind;
   
  fn main() {
      let f = File::open("hello.txt");
   
      let f = match f {
          Ok(file) => file,
          Err(error) => match error.kind() {
              ErrorKind::NotFound => match File::create("hello.txt") {
                  Ok(fc) => fc,
                  Err(e) => panic!("Problem creating the file: {:?}", e),
              },
              other_error => {
                  panic!("Problem opening the file: {:?}", other_error)
              }
          },
      };
  }
  
  ```
  
  We can also use [[Closure]]s and [[Unwrap]] to do the same
  For ex.:
  ```rust
  use std::fs::File;
  use std::io::ErrorKind;
   
  fn main() {
      let f = File::open("hello.txt").unwrap_or_else(|error| {
          if error.kind() == ErrorKind::NotFound {
              File::create("hello.txt").unwrap_or_else(|error| {
                  panic!("Problem creating the file: {:?}", error);
              })
          } else {
              panic!("Problem opening the file: {:?}", error);
          }
      });
  }
  ```
  
  However, the error's type itself will be different. Like ``io::Error`` is a [[Struct]] that contains all the ``io`` errors.
- Turning Result to panic
  Using [[Unwrap]] we can get just the value into a variable, as on ``unwrap`` it will panic/return some custom value.
- ``.expect("<string>")``
  [[Result Type]] also has this [[Method]] which prints the given message and aborts if ``Err``.
- Propagating Errors
  We can use [[Pattern Matching]] on a [[Result Type]] and return a [[Result Type]] again, this essentially *re-throws* or propagates the error back. 
  The ``?`` [[Operator]] is a shorthand for Returning an ``Err``, i.e., it returns an ``Err`` if Err is found and [[Copy or Move]]s the error object. If there's no Err then it does nothing.
  
  For ex.:
  ```rust
  fn yo()  -> Result<io::File, io::Error>{
  	 let x =  File::open("hello.txt");
  return match x{
          Ok(s) => Ok(s),
          Err(e) => Err(e),
      }
   
  }
  //is the same as
  fn yo2() -> Result<io::File, io::Error>{
  let x = File::open("hello.txt")?; //Returns an Ok into x, if there's an Err it returns the Err with its value immediately from the function
  
  Ok(x) //returns an Ok(x) 
  }
  ```
  
  * ``?`` vs. [[Pattern Matching]]
  A difference between these 2 is, ``?`` tries to coerce one error type into another. It does so by seeing what the expected return type of the current [[Function]]'s Result is then seeing if the error object has that type or its type has the ``From``[[Trait]] defined which converts the current error type into the return error type. If it is, then it calls the ``from`` [[Method]] of this [[Trait]] and does so.
  
  * This [[Operator]] can also be used with the [[Option Type]]
  For ex.:
  ```rust
  fn last_char_of_first_line(text: &str) -> Option<char> {
      text.lines().next()?.chars().last()
  }
  ```
  If there's a ``None``, return None otherwise continue.
- Main's return type
  The ``fn main`` [[Function]] returns the [[Unit Type]] by default, but it can also return a [[Result Type]]. For ex.:
  ```rust
  use std::error::Error;
  use std::fs::File;
   
  fn main() -> Result<(), Box<dyn Error>> {
      let f = File::open("hello.txt")?;
   
      Ok(())
  }
  ```
  ``Box<dyn Error>`` is a [[Trait Object]], here it means return any type of Error object. This main [[Function]] returns a 0 to the OS if Ok() and another value otherwise, the value returned is defined by the ``ExitCode`` of the ``Termination`` [[Trait]] of the Error's type.
-
-