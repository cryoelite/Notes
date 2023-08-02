- Concurrency, many tasks ran independently of each other. Parallelism, many tasks ran parallelly.
  title:: Concurrency or Parallelism
  Rust supports both types of asynchronous programming. The compile-time rules such as [[Ownership]], [[Borrow]] Checking, etc. help with async programming as well in Rust, making them less error prone.
- *Process*
  An OS runs a single program in a single *Process*, this is managed and scheduled by it as well. A single program can have multiple parts which run independently, these are known as [[Thread]]s.
- [[Channel]]s
- [[Mutex<T>]]
- [[Async and Await]]