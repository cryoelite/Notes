- Rust has no concept of *Classes* from other languages. It only has *Struct*, which is a collection of types much like a [[Tuple]] but each type is assigned a name. Each Struct is a unique [[Data Type]].
  
  Syntax:
  ``
  struct <name>{
  	<field_name>:<field_type>,
  	<field_name>:<field_type>,
  }
  ``
  Each name and [[Data Type]] pair in a struct definition is called a *field*.
  
  For ex.:
  ```rust
  struct User{
  	active: bool,
  	name: String,
  }
  
  let user= User {
    active: true,
    name: String::from("yo"),
  };
  
  let pp= user.active; //to access any field's value.
  ```
  After defining a struct, we can define values for its fields in an *instance*. We use the ``dot`` [[Operator]] to access each field's value.
- Just like [[Variable]]s, instances have ``mutability``.
  
  Every instance is an immutable instance by default, and we cannot modify any field’s value or the variable itself after initialization.
  
  To create a mutable instance we use ``let mut user = User{};``
  This is also to say, Rust doesn’t have mutable fields, the entire instance should either be mutable or immutable.
- Field init Shorthand
  Rust allows creating a struct without specifying field names if the [[Variable]] name is the same as the field it is being assigned to.
  
  For ex.:
  ```rust
  struct User {
  active: bool
  }
  
  let active=true;
  let user= User{
  	active
  }; //ok
  ```
- Struct update syntax
  
  A struct can be created from another struct without manually specifying each field,
  For ex.:
  ```rust
  struct User {
  active: bool,
  name: String,
  }
  
  let active= true;
  let user= User{
  	active,
      name:String::from("yo")
  };
  let user2= User{
      ..user
  };
  println!("{}",user.active);
  //If we also do println("{}", user.name); then it will be an error as user.name has lost ownership
  ```
  We can specify some *fields* if we want but we must specify them before defining the base struct. 
  Now it [[Copy or Move]]s the values from base struct into the new instance. The copy/move rules are followed the same. This makes the *field* of the base struct which lost [[Ownership]] in the move operation as invalid, however all the other fields can still be accessed.
- [[Tuple]] Struct
  We can create a struct which is exactly like a [[Tuple]] but just with a custom name and is a custom [[Data Type]] itself. 
  
  For ex.:
  ```rust
  struct Color(i32, i32, i32);
  struct AnotherColor(i32, i32, i32);
  
  //And to create an instance
  let color= Color(0,1,2);
  //and access 
  color.1; //like normal tuples.
  
  let another_color=AnotherColor(0,1,2);
  ```
  Here, ``color`` and ``another_color`` are not equal, even if they had the same values and fields, they are different types.
- Unit-Like struct
  Just like [[Unit Type]] which is just an empty [[Tuple]] , we have unit structs too.
  For ex.:
  ```rust
  struct Abc;
  let abc= Abc;
  ```
  and that’s it. It doesn't have much use.
- [[Reference Type]]fields
  We can have reference field types, like ``&str`` [[String]] , if we define [[Lifetime]]s . This is to ensure the struct owns the data until the instance exists to avoid bugs.
- [[Method]]
- [[Fully Qualified Syntax for Disambiguation]]
-