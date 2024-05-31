- A Symbol is an object used to represent other operators or identifiers. 
  It takes a string which remains constant inside the Symbol object. Symbols can be compared and 2 symbols with the same string inside them are equal, symbols stored by [[Reflection]] during [[Compilation]] can hence be compared too. 
  
  To create a symbol:
  ``#<any string no quotes needed>``
  or by using Symbol [[Object]]'s ctor 
  ``Symbol("<string>")``
  
  FE:
  ```dart
  void main() {
    var x = #abc;
    print(x); //prints Symbol("abc")
  }
  ```
  Symbol is kinda like a [[String]],i.e., it stores a string inside its object.
  It can be used to obtain information about an identifier at runtime as well, we can do so by using the symbol with a mirroring/reflection package which would store the identifier symbols at compile time and compare them at runtime as we can't directly get identifier information at runtime without reflection because of minification on their names.
-