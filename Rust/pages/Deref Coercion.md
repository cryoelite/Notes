- Rust can automatically convert a [[Reference Type]] of [[Data Type]] ``A`` into a reference of type ``B`` if the type ``A`` implements the ``Deref`` [[Trait]] and its ``deref()`` [[Method]] returns a value of type ``&B``. 
  This is how [[String]]'s ``&String`` is automatically converted into ``&str`` as the trait is implemented for ``String``.
  So if a [[Variable]]/ [[Function]]/ [[Method]]/etc. requires ``&B`` or in this case ``&str`` then we can pass ``&A`` or ``&String`` and Rust would automatically coerce the reference into that type using the dereference trait.
  
  For ex.:
  ```rust
  fn yo(x: &str) {}
  
  fn main() {
   let x = String::from("yo");
   yo(&x); //works
  }
  ```
- Rust can follow the deref chain so if ``&A``->``&B`` is ``&A``->``&C``->``&B`` then coercion will still work.
- Deref coercion is resolved at compile-time itself. So there is no runtime cost.