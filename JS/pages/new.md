- new.target
  Returns a bool which is true if the parent function was called with new.
  For ex.:
  ```js
  function Yo() {
   if(!new.target){
   return new Yo();
   }
   this.name = "yo";
  }
  
  let x = Yo(); //works
  ```
  This is unadvised.
- A Ctor [[Function]] call can omit the ``()`` if it doesn't take any params. 
  So ``let x = new Yo`` is valid syntax if ctor fn Yo exists. It is the equivalent of ``let x = new Yo()``.
-