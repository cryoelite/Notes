- Lifetimes are kind of like [[Generic Type]]s and whilst Rust implicitly uses them, we can also use them as abstract symbols that explicitly define the validity of [[Reference Type]]s. 
  Just like the [[Data Type]] for [[Generic Type]]s is inferred automatically, the lifetime for [[Reference Type]]s are also inferred automatically.
  
  The main objective of lifetimes is to track the validity of references making sure they are valid when accessed.
  
  Lifetimes are always used for [[Reference Type]]s, the only thing is that Rust can infer the lifetimes automatically most of the time (aka *Lifetime Elision*), but where it can't, we have to explicitly define the lifetime.
- The [[Borrow]] Checker
  Rust compiler has this feature, which allows it to check, at compile time itself, if all the references in the code are valid, checking if the [[Borrow]] is valid. 
  
  For ex.:
  ```rust
  fn main() {
      let r;                // ---------+-- 'a
                            //          |
      {                     //          |
          let x = 5;        // -+-- 'b  |
          r = &x;           //  |       |
      }                     // -+       |
                            //          |
      println!("r: {}", r); //          |
  }                         // ---------+
  ```
  Here, the lifetime of ``r`` is represented with ``'a`` and for ``x`` it is ``'b`` and as we can see ``'b`` is valid for a smaller duration than ``'b`` and hence if the reference ``r`` tries to access the value of ``x``, it will fail as the life of ``x`` is over already.
  The Borrow Checker checks all borrows like so at compile time.
  
  Similarly, in [[Function]]s/ [[Method]]s , if we use [[Reference Type]] returns, and when it is not inferrable which parameter will be returned, we need to explicitly define the lifetimes. 
  For ex.:
  ```rust
  fn longest(x: &str, y: &str) -> &str {
      if x.len() > y.len() {
          x
      } else {
          y
      }
  }
  
  fn main() {
   let a= "yo";
   let mut c: &str;
   { 
    let b= "no";
    c = longest(&a,&b); //Which reference is returned from longest ? 
   } //if c has the reference lifetime of b then this is where it becomes invalid 
   
  } //if c has the reference lifetime of a then this is where it becomes invalid 
  ```
  This code leads to an error at compile time, because the borrow checker is not sure if ``x`` or ``y`` will be returned, meaning it is not guaranteed for the caller of ``longest()``which lifetime it will have. 
  This is why we need to explicitly define the lifetimes for ``longest`` here to let the compiler know which lifetime is being returned.
- Explicit definition of lifetimes
  We use the same syntax as [[Generic Type]]s, however the key difference is that generic lifetime names need to be prefixed with the ``'`` letter, like ``'x`` and they aren't [[Generic Type]]s so they can't be used to replace them in the syntax.
  
  Syntax for [[Function]]s/ [[Method]]s :
  ``
  fn <fn name><< ' + lifetime name 1>, < ' + lifetime name 2>,..., < ' + lifetime name n>(<param name 1>: <if param type & then, & ' + lifetime name>, ...)-> <if return type & then, & + ' + lifetime name> {...}
  ``
  Lifetime definitions on [[Function]] parameters are called *input lifetimes* while the return type lifetime definitions are called *output lifetimes*.
  
  For ex.:
  ```rust
  fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
      if x.len() > y.len() {
          x
      } else {
          y
      }
  }
  ```
  This function has only a single lifetime generic, so it will only infer a single lifetime from its args and expect the same from both the args. If the args have different lifetimes, then the smaller lifetime is inferred. 
  Here, we are not changing the lifetime of ``longest``, rather only allowing certain lifetimes and letting the [[Borrow]] checker know which lifetimes will be used where. 
  
  We can only define lifetimes in [[Function]] Signatures, not in their bodies.
  Furthermore, when returning a reference type and defining a lifetime for it, the lifetime must be also applied to a reference parameter.
  
  * For [[Struct]]s:
  For ex.:
  ```rust
  struct X<’a> {
  	yo: &’a str,
  }
  //says the instance of struct X will only live as long as the string field yo lives.
  ```
  
  * For [[Method]] implementation blocks:
  ```rust
  impl<’a> X<’a> {
  	fn yo(&self) {…}
  }
  ```
- *Lifetime Elision* Rules
  [[Function]]s and ``impl`` blocks always need a lifetime definition for [[Reference Type]]s. 
  
  We don't always need to explicitly define them, but if none of these rules can be applied then they need to be explicitly defined:
  *	Each reference parameter in signature gets a separate lifetime. 
  ```rust
  fn yo(x: &str) {} //gets fn yo<’a>(x:&’a str) 
  fn yo2(x: &str,y: &str){} //gets fn  yo<’a, ‘b>(x: &’a str, y: &’b str) 
  //and so on
  ``` 
  
  *	If there’s exactly 1 reference parameter and it is given an explicit lifetime then the same lifetime is applied to the return type reference, if any.
  * If there are multiple *input lifetime* parameters, and one of them is ``&self`` or ``&mut self`` then the ``self``’s lifetime is applied to the return type reference. 
  
  Multiple rules will be applied to the same signature if a single rule doesn’t make the signature valid.
  For ex.:
  ```rust
  Struct X{}
  impl  X {
  	fn yo(&self, x: &str) -> &str {…} 
  }
  // This is not an error, as the 1st rule applies a lifetime to both the parameters and the 3rd rule applies the self’s lifetime to the return type. 
  ```
- *Static lifetime*
  Applying this lifetime to a [[Reference Type]] puts it in the binary and increases the size of it. But the reference is valid for the duration of entire program. 
  Static lifetime can only be applied to values that are known at compile time.
  For ex.:
  ```rust
  let x: &’static str= "yeee";
  ```
  
  All [[str]] literals have the static lifetime anyways.
-