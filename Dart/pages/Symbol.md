- A Symbol is an object used to represent other operators or identifiers. 
  It takes a string which remains constant inside the Symbol object, this string can be used to link the Symbol to an identifier as well, in which case it becomes the static representation of that identifier even after [[Compilation]] (because compilation minifies identifier names hence changing them).
  
  FE:
  ```dart
  var x = 
  ```