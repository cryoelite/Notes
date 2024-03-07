alias:: str
Not really an alias, as String and str are different things but the term is used interchangeably.

- Collection of bytes, the Collection itself provides many features on the underlying bytes and interprets them as text. At the core of rust, lies only a single type, the ``str`` [[Data Type]], then the [[Standard Library]] provides the ``String`` [[Data Type]] collection.
  
  The ``str`` type is a string [[Slice]] so it represents a fixed-size string whereas the ``String`` type is growable. There are various methods to convert value from either type into the other.
  
  Despite intuition, a ``String`` is *NOT* a collection over [[Char]]s. Infact, [[Char]]s have nothing to do with strings (be they ``str`` or ``String``).
- ``String``
  Growable, mutable and [[Owned]] Collection which parses its bytes as UTF-8 strings.
  The ``String`` type is simply a wrapper over [[Vector]] of bytes but with certain restrictions, guarantees and features.
  
  To create a new String we use ``String::new()`` much like a Vector.
  For ex.:
  ```rust
  let x= “abc”.to_string();
  let mut y=String::new();
  y=”abc”.to_string();
  let z= String::from(“abc”);
  ```
- ``str``
  An ``str`` is just a [[Slice]] of a string meaning it is a fixed-size immutable string slice. It is also called a String literal as the ``str`` can only be declared at compile-time. However, it's [[Reference Type]] ``&str`` can be created at any time.
  
  For ex.:
  ```rust
    let value = 3;
      let yo = match value {
          1 => "1",
          other => {
              let someOtherValue = other.to_string();
              &someOtherValue[..]
          }
      };
  ```
  This code results in an error as the ``&str`` from ``1`` does not have the same [[Lifetime]] as the ``other`` arm in the [[Pattern Matching]]. The first arm has an ``&str`` with static lifetime as the str is known at compile time making its reference ``&str`` have a static lifetime but the latter is only known at run-time making its lifetime smaller than the other arm. We can fix it like so
  ```rust
      let value = 3;
      let _yo = match value {
          1 => "1".to_string(),
          other => {
              let someOtherValue = other.to_string();
              (&someOtherValue[..]).to_string()
          }
      };
  ```
  So now both arms return a ``String`` [[Data Type]]. 
  
  
   
  ``"abc".to_owned()`` [[TODO]] also define it all better here
- Conversion between ``String`` and ``str``
  To convert a ``String`` to a ``str`` we use the [[Slice]] [[Operator]] like so 
  ```rust
  let s= String::from("axb");
  let x = &s[..];
  ```
  ``x `` is of type ``&str`` here.
  
  And to convert an ``str`` to ``String``, we can
  ```rust
  let s= "x";
  let y= String::from(s); //ok
  let z= x.to_string(); //ok
  ```
  We can also use ``&str`` [[Reference Type]] with these [[Function]]s.
  
  * ``.as_string()``
  Gets a ``String`` as an ``&str``, equivalent to ``&<String object>[..]`` [[Slice]].
- ``.push_str(<String>)`` or ``.push(<Char>)``
  Appends a String or [[Char]] to a ``String`` object.
  
  * Concatenation
  A bit different in Rust due to [[Ownership]] rules. 
  ``String`` can't be concatenated to another ``String``, it can only be concatenated to an ``&str``, or ``&String`` which would be automatically coerced into ``&str`` using [[Deref Coercion]].
  For ex.:
  ```rust
  let s1= String::from("yo");
  let s2= String::from("na");
  let s3= s1+ &s2; //works but s1 loses Ownership of its data.
  ```
  This is because the ``+`` Operation is as ``Add`` [[Method]] on any custom [[Data Type]] in Rust and the ``Add`` method for String looks like so 
  ```rust
  fn add(self, s: &str) -> String 
  ```
  
  In other words, every string concatenation requires at-least one ``String``, and then there can be any number of ``&str``s and we can concatenate them.
  For ex.:
  ```rust
      let s1 = String::from("tic");
      let s2 = String::from("tac");
      let s3 = String::from("toe");
  
      let s = s1 + "-" + &s2 + "-" + &s3;
  ```
  Intuitively, this makes sense because Rust needs to know of a place where to store the concatenated String, if there are only immutable [[Reference Type]]s then it doesn't have that, if it has more than one  ``String``s then it's not sure which String to use, and even if it is, the developer won't be sure which one it is and would lose [[Ownership]].
  
  * The ``format!(<String>)`` [[Macro]]
  This macro is used for String concatenation as it returns a new ``String`` with all the strings concatenated.
  For ex.:
  ```rust
  let s1= String::from("yo");
  let s2= String::from("yoaa");
  let s3= "as";
  let s3= format!("{s1} and {s2} {s3}"); //works
  ```
  Since it creates a new ``String``, it doesn't need the [[Ownership]] of any ``String`` and hence it takes the immutable [[Reference Type]] of all the strings passed to it.
- Indexing
  Integer indices to access characters (not [[Char]]) in a string in Rust isn't valid. This is because each character is UTF-8 encoded. 
  A ``String`` is a wrapper over ``Vec<u8>``, but UTF-8 requires 1 to 4 bytes (4*8 = 32 bits) per character, this variable size is by UTF-8 spec. This means it is not sure if a given index is a single character or a part of a multi-byte character and hence accessing ``String`` by index is not guaranteed to return the right character. 
  For ex.:
  ```rust
  let hello = "Здравствуйте";
  let answer = &hello[0]; //is an error
  ```
  This doesn't compile in Rust as rust avoids such errors and here the error would be correct, as we are trying to get the first character, which we would expect would be the Cyrillic letter ``Ze`` and not Arabic Number ``3``, it takes 2 bytes according to UTF-8 and hence both index 0 and 1 make up the character ``Ze``. 
  
  We can use ``&hello[0..2]`` (end exclusive) and that would work as string [[Slice]]s are allowed anyway. But String [[Slice]]s are risky, if the start or end index of a string slice bisects a character, it [[Panic]]s. 
  
  We can see why [[Char]] isn't used to represent characters in ``String`` as string slice from 0 to 1 above would return a string with the right character, which has taken 2 bytes and Rust can't know if the string slice from 0 to 1 is a single character or it is a string.
  
  * There are 3 ways Rust sees characters in String. 
  For “नमस्ते”,
  
  It’s length is 18 in bytes and this is how it looks as a byte-array,
  [224, 164, 168, 224, 164, 174, 224, 164, 184, 224, 165, 141, 224, 164, 164, 224, 165, 135]
   
  Rust sees chars as Unicode Scalar Values, basically like this 
  ['न', 'म', 'स', '्', 'त', 'े']
  The 4th and 6th letter here are called *diacritics* as they don’t make sense on their own. 
  
  Lastly, here’s how we interpret these letters,
  ["न", "म", "स्", "ते"]
  These are the *Grapheme Clusters* of the word Namaste.
  A grapheme cluster is a more accurate name of what we call a letter in English. 
  
  This is why indexing is not done in Rust. This is also why, [[Slice]] operations are risky, for ex.:
  ```rust
  let hello = String::from("Здравствуйте");
  //hello[0..1]; //will cause the program to panic as the first byte is a part of 2 to make up a Unicode Scalar value, so 
  hello[0..2] //works
  ```
-
- Raw string
  Get a string as defined, we use ``"\ <string, can be multiple lines> ";``
  
  For ex.:
  ```rust
  let contents = "\
  Rust:
  safe, fast, productive.
  Pick three.
  Duct tape.";
  
  ```
  or use ``r#" <string> "#``
  For ex.:
  ```rust
  let abc= r#"my string"#;
  ```
-
-