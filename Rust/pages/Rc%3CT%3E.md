- Aka *Reference Counting*. This [[Data Type]] is a [[Pointer]] that is meant to give [[Ownership]] to multiple owners. Like a graph node might have multiple parents.
  
  Defined in ``std::rc::Rc`` [[Module]] in the [[Standard Library]]
  
  For ex.:
  ```rust
  use std::rc::Rc;
  
  fn main() {
   let a= Rc::new(10);
   let b = Rc::clone(&a); //is the same as a.clone();
  }
  ```
  We use the ``clone()`` [[Method]] in the ``Rc`` [[Struct]] directly instead of using the ``.clone()`` instance method just for convention. It helps show that the Rc's clone is called and not a normal clone as on other types, they do the same thing for ``Rc``. The reason we do this is also because a normal clone on other [[Data Type]]s deep copies the values, but for an ``Rc``, this method returns an ``Rc`` and internally points to the same [[Reference Type]]. It also mutably increases an internal reference count to know how many references of the data are alive.
  The ``Drop`` [[Trait]] then reduces the references counter and when it reaches 0, clears up the data.
- Strong Reference ``strong_count`` and  Weak Reference``weak_count``
  Just like *Shared Pointer* in *C++* has a weak and a strong reference, so does ``Rc``. The strong count prevents cleanup of the data whereas weak count doesn't so if we have a weak clone of ``Rc`` , it may become have an invalid [[Reference Type]] if the strong count drops to 0.
  
  Using ``Rc::clone(&<Rc instance>)`` we get a strong reference of type ``Rc<T>``, and that shares the [[Ownership]] of the data within, it also bumps the ``strong_count`` by 1. With ``Rc::downgrade(&<Rc instance>)``  we instead get a value of type [[Weak<T>]] and the ``weak_count`` of ``Rc`` is increased by 1. 
  Since it is not guaranteed if the value referenced by the [[Weak<T>]] will be valid, we have to explicitly check. We do so with ``<Weak<T> instance>.upgrade()`` which returns an [[Option Type]] ``Option<Rc<T>>`` which is ``Some`` if the reference is still valid or ``None`` otherwise. 
  
  
  * The ``Rc::strong_count(&<Rc Type instance>)`` returns the strong count of the given ``Rc`` instance.
  
  * Similarly, ``Rc::weak_count(&<Rc Type instance>)`` returns the weak count.
  
  For ex.:
  ```rust
  use std::rc::Rc;
  fn main() {
      let a= Rc::new(2);
      let b= Rc::downgrade(&a);
      let c= b.upgrade();
      match c {
       Some(value)=> println!("{}", *value), //prints 2
       None=> println!("na");
      };
  }
  ```
-
-