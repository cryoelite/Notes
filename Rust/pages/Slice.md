- A *slice* is a [[Reference Type]] to a contiguous segment of elements of a [[Collection]].
  Since it is a reference type, it doesn't have any [[Ownership]] of the values.
  
  For ex.: For the [[String]], 
  ```rust
  let s = String::from(“Hello”);
  let x= &s[..];  //entire string
  let y= &s[..5]; //index 0 to 5, end exclusive
  let z = &s[2..]; //index 2 to the end exclusive
  //all of these have the type &str
  let x = &mut s[..];  //type is &mut str
  ```
  For strings specifically, the indices must be valid and they must represent valid byte [[Char]]s ends, that is, the range mustn't bisect any multi-byte character in UTF-8. And only for types like ``String``, taking a slice returns a ``&str``. For other types, the slice returns ``&<T>[ ]`` [[Data Type]]. 
  
  ``&str`` is specially useful in [[Function]]s and [[Variable]]s , because not only can they take slices, but also ``String``s.
  For ex.:
  ```rust
  let p= String::from(“hello”);
  let x: &str = &p; //works as &p is coerced into &p[..]. 
  ```
  This is due to [[Deref Coercion]].
- Slices on [[Array]]s 
  For ex.:
  ```rust
  let a = [1,2,3,4,5];
  let b = &a[..]; //b has the type &[i32].
  ```
-
-
-