- In Rust, an iterator is a [[Trait]] that [[Loop]]s over the items of a sequence. 
  They are lazy, so until the next value is requested in a sequence, they don't iterate over it.
  
  For ex.:
  ```rust
  fn main() {
  let list= vec![1,2,3];
  let listIt = list.iter();
  
  for item in listIt {
  	//use item
  }
  
  }
  ```
- ``Iterator`` [[Trait]]
  Any [[Data Type]] that implements this trait, gets [[Method]]s such as ``.into_iter()`` and ``.iter()`` which return an iterator.
  
  This trait is defined like so
  ```
  pub trait Iterator {
  type Item;
  
  fn next(&mut self) -> Option<Self::Item>;
  // the other methods have default implementations 
  }
  ```
  Here, the ``type Item`` is an [[Associated Type]]. 
  So if our [[Data Type]] implements this trait then it must provide a type to ``Item`` and the ``next`` [[Method]] must be defined.
  
  The way it works is simple, ``next`` uses mutable [[Borrow]] to get the next value of the data type and then change the internal state of the iterator marking the current position. This is why iterators are exhaustive, i.e., if an iterator instance is created and the ``.next()`` called then the iterator will reach a terminal state and won't start over again. This is required behavior too, as the ``.next()`` returns an [[Option Type]] so it must return ``Some`` if there's a value or ``None`` if it is exhausted.
  
  For ex.:
  ```rust
  fn main() {
      let list = vec![1, 2, 3];
      let mut listIt = list.iter(); //Iterators change the internal state, so the variable must be mutable
      let x: &i32 = &1220;
      println!("{}", listIt.next().unwrap_or(x));
      println!("{}", listIt.next().unwrap_or(x));
      println!("{}", listIt.next().unwrap_or(x));
      println!("{}", listIt.next().unwrap_or(x)); //prints 1220
  }
  
  ```
  
  The ``Some()`` returns an immutable [[Reference Type]] to the value in the actual sequence container when we get an iterator with ``.iter()``. We can get mutqable references with ``.`iter_mut()`` and take [[Ownership]] of the values with ``.to_iter()`` so now the value is [[Copy or Move]]ed into the ``Some()``.
  
  * The ``for`` [[Loop]] doesn't need the [[Variable]] to be ``mut`` as it can internally do that.
  For ex.:
  ```rust
  fn main() {
    let list = vec![1, 2, 3];
    let list_it= list.iter();
   for item in list_it{
     //works, doesn't need list_it to be mutable.
   }
  }
  ```
  
  * Iterator Trait [[Method]]s that call ``.next()`` are called *Consuming Adaptor*s as they consume up the iterator.
  On the contrary, we have *Iterator Adaptor*s, which are methods that don't consume up the iterator and instead produce different iterators. 
  For ex.:
  ```rust
  fn main() {
    let list = vec![1, 2, 3];
    let new_it= list.iter().map(|x| x + 1); // is an iterator that will take a value from list, apply the map closure, then  return the new value. 
  ```
  using a [[Closure]]. 
  * ``.collect()`` on an iterator consumes it up and returns a Vector.
  For ex.:
  ```rust
   let list = vec![1, 2, 3];
   let collected_it : Vec<_> = list.iter().map(|x| x + 1).collect(); 
  }
  ```
  Using the ``placeholder`` type and [[Vector]].
- Iterators vs [[Loop]]s
  Iterators follow the *zero-cost abstraction principle* in Rust. 
  "What you don’t use, you don’t pay for. And further: What you do use, you couldn’t hand code any better.” - Bjarne Stroustrup
  
  This basically means unlike loops, iterators will always be the most efficient ways of iterating over sequences. They will always be equal, if not faster, than raw loops. So it is recommended to use iterators wherever loops are concerned. This is because iterators are unrolled. Unrolling is an optimization that compilers perform by unwrapping fixed size loops into individual pieces of code, so if a loop runs 12 times then 12 similar definitions are created. This increases the binary size but allows the runtime to avoid bound checking, and other overheads associated with loops.
-
-