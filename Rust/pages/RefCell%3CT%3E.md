- Much like [[Rc<T>]]. However it uses the [[Interior Mutability Pattern]] to do something other [[Pointer]]s can't. It allows immutable references from mutable [[Reference Type]]s. 
  Defined in the ``std::cell::RefCell`` [[Module]] in the [[Standard Library]].
   
  For ex.:
  ```rust
  use std::cell::RefCell;
  fn main() {
      let a= RefCell::new(2);
      let b= &a;
      let mut c= b.borrow_mut();
      *c=3; //works
  }
  ```
  
  The ``.borrow()`` [[Method]] returns a ``Ref<T>`` instance and ``.borrow_mut()`` returns a ``RefMut<T>`` instance, this instance has mutable [[Reference Type]] to the data inside ``RefCell``.
- ``RefCell`` keeps a track of all its immutable and mutable [[Reference Type]]s like [[Rc<T>]].
  This allows there to be any number of immutable references, or a single mutable reference to a value at any time. This is exactly like [[Reference Type]] mutability rules. However, a big difference is that instead of the compiler failing to compile code that violates this rule, we get [[Panic]] at runtime instead.
-