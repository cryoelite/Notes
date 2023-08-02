- These statements allow simple branching. Unlike languages like JS, non- [[Bool]]s aren’t automatically converted to [[Bool]]s .
- If-else if-else
  The ``if``/``else if``/``else`` blocks, these blocks are also called ‘arms’.
  Syntax:
  ``if x< y {…}
  else if x> y {…}
  else {…}``
  
  * if is an expression. So it returns a value. Hence we can do this,
  ```rust
  let z = if 2<3 { 2 } else { 3 };
  ```
  However, if we return values of differing types then it is an [[Error]] as both arms have to return values of same [[Data Type]] and that type must be assignable.
- if let-else if-else
  Uses [[Pattern Matching]], it's a shorter form of ``match`` expressions, as it only cares about given arms.
  Unlike match arms which covers all cases, matches one case, if-let covers one case and ignores all the rest. We can still have multiple arms with ``if let-else if-else``.
  Syntax:
  ``
  if let <Pattern> = <variable/value> { }
  else if <Pattern> = <variable/value> { }
  else {}
  ``
  
  For ex.:
  ```rust
  fn main() {
   let x= Some(2);
   if let Some(value) = x { //Pattern matching
    //do something with value
   } //that's it, it implicitly does _ => () to ignore all the rest cases.
  
   let y =2;
   if let 3 = y {
    //something
   }
   else if let 2 = y {
    //ya
   }
   else {
     //...
    }
  }
  ```
  
  * Unlike ``match``, if let can even use pattern matching against different variables/values in each arm.
-
-