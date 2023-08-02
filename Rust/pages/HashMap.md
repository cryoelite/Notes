- ``HashMap<K,V>``
  Defined in the [[Standard Library]], this [[Struct]] provides an implementation of a hashmap, or hashtable etc. from other languages where the Key is *hashed* and stored and the value is also stored with it. The benefit of this [[Data Structure]] is efficient search of the key.
  
  For ex.:
  ```rust
    use std::collections::HashMap;
  
  fn main() {
       let mut scores = HashMap::new();
      scores.insert(String::from("Blue"), 10);
     let team_name= String::from("Blue");
      let score = scores.get(&team_name).copied().unwrap_or(0);
  }
  ```
  
  Here ``.copied()`` is a [[Method]] on the [[Option Type]] which takes ``Option<&V>`` and returns an ``Option<V>`` by [[Copy or Move]]ing its value and finally [[Unwrap]] unwraps the ``Option`` and returns a 0 if it is ``None``.
- Getting/Setting values in the HashMap follows the [[Ownership]] rules, this is why it is recommended to use [[Reference Type]]s.
- Inserting
  If a key exists then its value is overwritten. If  we want to check before inserting we can use the ``.entry`` method,
  For ex.:
      ```rust
  let mut scores = HashMap::new();
  scores.insert(String::from("Blue"), 10);
   let insertedValue= scores.entry(String::from("Yellow")).or_insert(50);
  
  ```
  Here ``.entry()`` returns an [[Enum]] ``Entry`` which defines if the key exists or not. The Entry enum also defines a few [[Method]]s and one of them is ``.or_insert(<V>)``, which inserts the key and the value if the key doesn’t exist otherwise does nothing. Either way, ``.or_insert`` returns a mutable [[Reference Type]] of the value to the key.
- Some other methods
  For ex.:
  ```rust
  use std::collections::HashMap;
   
  let teams = vec![String::from("Blue"), String::from("Yellow")];
  let initial_scores = vec![10, 50];
   
  let mut scores: HashMap<_, _> =
  teams.into_iter().zip(initial_scores.into_iter()).collect();
  ```
  ``.zip`` takes 2 [[Iterator]]s and pairs each value at the same index of them into a [[Tuple]] and ``.collect`` collects the tuples and then returns a required type which is HashMap here. Since collect needs a type to know what to return to, and HashMap needs types to define K and V, Rust allows us to use ``_`` the Placeholder type [[Operator]] for the [[Generic Type]]s. Then it infers the correct types.
-