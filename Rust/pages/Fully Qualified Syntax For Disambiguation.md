- We can define [[Method]]s and [[Trait]] methods with same names for a [[Data Type]] as long as they aren’t defined in same [[Scope]]. Then we call them with their full syntax. 
  
  For ex.:
  ```rust
  struct Human {}
  
  trait Pilot {
      fn fly();
  }
   
  trait Wizard {
      fn fly(&self);
  }
    
  impl Pilot for Human {
      fn fly() {
          println!("This is your captain speaking.");
      }
  }
   
  impl Wizard for Human {
      fn fly(&self) {
          println!("Up!");
      }
  }
   
  impl Human {
      fn fly(&self) {
          println!("*waving arms furiously*");
      }
  }
  
  fn main() {
   let x= Human{};
   x.fly(); //prints waving arms furiously
   Wizard::fly(&x); //prints up
   <Human as Pilot>::fly(); //prints This is your captain speaking
  }
  ```
  Since ``fly()`` is ambiguous, Rust defaults to calling the [[Method]] on the [[Data Type]] directly, if it is not available then that's an error. 
  We have to explicitly call the [[Method]] that we wish to invoke in ambiguities like these. 
  Methods that take ``self`` as a parameter can be explicitly given an instance, given the [[Data Type]] of instance implements the type for which the method is called. 
  
  In case of [[Method]]s without ``self`` we would need to [[Cast]] the [[Data Type]] , we can do so using [[as]].