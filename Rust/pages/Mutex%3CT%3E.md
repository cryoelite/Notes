- ``Mutex<T>``
  A mutex, stands for ``Mutual Exclusion`` is a concept that allows the same region of memory to be accessed by multiple [[Thread]]s. The way mutex achieves this is by *guarding* the data by only allowing a single thread to be able to access it at once.
  
  For a thread to access data behind a mutex, it must 
  * First acquire the lock to the mutex, this will be immediate if no other thread is using it, or the thread will have to wait till the using thread unlocks it.
  * Then it must unlock the mutex once it is done using it.
  
  In Rust, it is defined in ``std::sync::Mutex`` and is a Smart-[[Pointer]] 
  For ex.:
  ```rust
  use std::sync::Mutex;
  
  fn main() {
      let m = Mutex::new(5); //type of m is Mutex<i32>
  
      {
          let mut num = m.lock().unwrap(); //num's type is MutexGuard
          *num = 6;
      }
  
      println!("m = {:?}", m); //prints 6
  }
  ```
  
  ``.lock()`` returns an [[Enum]] like [[Result Type]] called ``LockResult``, which returns an ``Err`` if acquiring the lock fails, which can be if the thread that originally had the lock [[Panic]]ked or died without unlocking the mutex. It's ``Ok(T)`` equivalent is a [[Data Type]] [[MutexGuard<T>]] which is a smart- [[Pointer]] to the data behind the mutex.
  
  [[MutexGuard<T>]] implements the ``Drop`` [[Trait]] to drop the lock of the mutex along with itself.
  
  As we can see, mutex in Rust uses the [[Interior Mutability Pattern]]
- Passing Mutex to multiple [[Thread]]s
  This requires another smart [[Pointer]] as the [[Ownership]] to the ``Mutex`` is required for a thread to acquire a lock to it, and moving it to a different thread in a [[Closure]] is a one-way thing.
  Furthermore we have to use pointers that can be safely shared across threads, such as [[Arc<T>]].
  
  For ex.:
  ```rust
  use std::sync::{Arc, Mutex};
  use std::thread;
  
  fn main() {
  let a= Arc::new(Mutex::new(3));
  let b= Arc.clone(&a);
  thread.spawn(move|| {
  	let mut c= b.lock().unwrap();
  	*c= 5;
  })
  println!(“{}”, *b.lock().unwrap());
  }
  ```
-