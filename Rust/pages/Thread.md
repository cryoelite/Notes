- Rust follows a 1:1 threading model, meaning 1 Rust thread will take exactly 1 thread in the OS.
  
  Defined in ``std::Thread``.
  For ex.:
  ```rust
  use std::{thread, time::Duration};
  fn main(){
      
      let handle= thread::spawn(|| {
          println!("yo");
          for i in 1..20 {
              println!("{}",i);
          }
      });
   
      println!("enddd");
      thread::sleep(Duration::from_millis(10));
   }
  
  ```
  * ``thread::spawn(<closure>)`` to spawn a thread and execute the given [[Closure]]. 
  
  Returns a ``JoinHandle<T>`` [[Data Type]]. Here ``T`` is the return type of the closure, so if nothing is returned by the closure it is the [[Unit Type]].
  
  * ``JoinHandle`` has a ``join()`` [[Method]] which stops the current thread's execution until the thread handled by the handle finishes execution.
  The thread that spawns another thread does not wait for the other thread to finish, and if it's the *Main Thread* that finishes without waiting for all its spawned threads then it might die before them and the OS kills the process when the *MT* dies so they may never finish as all of them are cleared up.
  
  ``join()`` returns an [[Result Type]] which represents the value of closure if the thread finishes successfully and an error otherwise. 
  
  For ex.:
  ```rust
  use std::{thread, time::Duration};
  fn main(){
      
      let handle= thread::spawn(|| {
          println!("yo");
          for i in 1..20 {
              println!("{}",i);
          }
      });
     handle.join().unwrap(); //waits and returns the value of the closure.
      
      
  
   }
  ``` 
  
  * [[Closure]]'s passed to other threads must be ``move``d if they capture some variable from thread, this is because it may be that the [[Lifetime]] of the [[Variable]] is over before the spawned thread can use the captured value. 
  * ``thread::sleep(<Duration instance>)`` to sleep a thread with the given ``Duration``.
-
-