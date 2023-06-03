- [[Object]] wrapper for primitive type ``symbol``
- A ``symbol`` represents a unique identifier in JS.
  For ex.:
  ```js
  let x = Symbol("yoo");
  ```
  The "yoo" is the symbol's description(aka Symbol name), it is optional but helps in debugging.
  We can get the description of a Symbol with the ``.description`` property.
- Symbols are always unique, that is 2 symbols are always inequal, even if they have the same description.
  ```js
  let x= Symbol();
  let y= Symbol();
  x==y; //false
  ```
- Symbols are the only type that don't implicitly get converted to another type.
  So if we call ``alert(...)`` with a Symbol, it will give an error. We can explicitly convert a Symbol to a string with ``.toString()`` method of the Symbol.
- Since Symbols are always unique, they are meant to be used as unique property names for [[Object]]s. This way we never overwrite any pre-existing Object property.
  ```js
  let x= Symbol("id");
  let y= {...};
  y[x]= "yo";
  y["id"]= "abc";
  
  y[x]; //returns "yo"
  
  //To use a Symbol inside the Object declaration,
  let z= {
    [x]: "yo", //we use this special [<Symbol>] syntax.
  }
  ```
- Global Symbol Registry
  Since Symbols are always unique, sometimes we may need Symbols properties to be accessible even outside the scope of the variable holding the Symbol.
  To achieve this we can use the Global Symbol Registry which stores Symbols and these symbols are called ``Global Symbols``.
  
  To create a symbol in the GS, if it doesn't already exist 
  ```js
  let x = Symbol.for("yo");
  ```
  Creates a Symbol with the "yo" key, if it doesn't already exist in the GS. If it does exist, it simply returns it, otherwise it creates it, stores it in the GS and returns it to x.
  
  If we wish to check if a Symbol with the given key/description exists in the GS, we can use ``Symbol.keyFor(<symbol>)``. It returns undefined if the Symbol with the same key isn't found in the GS, otherwise it returns the Symbol's key/description
  For ex.:
  ```js
  let s= Symbol("id");
  Symbol.keyFor(s); //returns undefined
  ```
-
- There are various other Symbols, that can be used to fine-tune [[Object]]s.
  They are known as [System Symbols](https://tc39.es/ecma262/#sec-well-known-symbols).
-