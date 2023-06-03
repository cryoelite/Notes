- This keyword is special in JS and is not evaluated until runtime.
- It is not bound to any construct in [[ECMAScript]] either, i.e., it can be used anywhere.
  For ex.:
  In an [[Object]]
  ```js
  let x ={
   age:2,
  };
  function bobo() {
    return this.age;  
   }
  
  x.yo= bobo; 
  x.yo(); //returns 2
  
  bobo(); //error as 'this' is undefined. Here 'this' is called unbound.
  ```
  In [[Old Mode]] an unbound this refers to the parent object, such as in a browser JS script if the function is global then the unbound this will refer to the [[window]] object.
- Arrow [[Function]]s have no context so they don't bind a this. Instead they allow ``this`` to reference the outer [[Scope]].
  For ex.:
  ```js
  let x= {
   name: "aa",
    yo() {
      let a= () => {console.log(this.name)};
    }
  };
  
  x.yo(); //works, prints aa
  ```
-