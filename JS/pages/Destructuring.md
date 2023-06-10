alias:: Destructuring Assignment

- This syntax allows us to destructure [[Array]]s and get them into variables.
  For ex.:
  ```js
  let arr= ["abc",1];
  let [a,b]= arr;
  //works
  ```
- If we want to omit a value, we leave an empty ``,``
  For ex.:
  ```js
  let arr= ["abc",1,2];
  let [a,,b]= arr;// ok
  
  ```
- Specifically, any [[Iterable]] can be destructured. It's like for..of [[Loop]].
  For ex.:
  ```js
  let [a,b,c]="abc";  //ok
  ```
- Can be used to swap variables
  ```js
  let a= 2;
  let b ="A";
  [a,b]= [b,a];
  ```
- If the [[Iterable]] is longer than the number of variables in the destructuring assignment, then the extra elements are simply ignored.
  
  Similarly, if the Iterable is shorter than the number of variables, then unassigned variables are simply [[undefined]].
  
  We can give a default value to variables too
  ```js
  let arr= ["abc",1];
  let [a,b,c = "yo"]= arr; //ok, c will be yo since it is unassigned after destructure assignment
  ```
  And just like normal default values in [[Function]]s, the default value can be a function too and it is called when the variable is unassigned.
- The ``...`` [[Operator]] can pick up the rest of the elements and becomes an [[Array]] holding those elements.
  That is,
  ```js
  let arr= ["abc",1,"b"];
  let [a,...b]= arr;  //b[0] is 1 and b[1] is "b" now.
  ```
- Works with raw [[Object]]s too.
  However, the destructuring requires the property names to be given. 
  That is,
  ```js
  let x= {
   "a":2,
   b:3,
  };
  let {a,b} = x; //ok 
  // we can also
  let {x: a, y: b}= x; //Here the syntax is variable name: source name
  //and also add default values
  let {a1: a= 5, a2: b = prompt("What's b ?") }= x;
  //also with ...
  let {...z}= x; //ok, z is not an Array but an Object.
  z[a]; //returns 2
  ```
  However, there's 1 thing we can't do exactly like Array Destructuring, that is declaration and destructuring separately,
  ```js
  let x= {...};
  let a;
  let b;
  {a,b} = x; //error
  ({a,b} = x); //ok
  ```
  This is because the syntax closely resembles a code block and for a code block it is invalid syntax, by using (...) we override that behavior.
- [[Object]] Destructuring can also use Nested Destructuring.
  For ex.:
  ```js
  let options = {
    size: {
      width: 100,
      height: 200
    },
    items: ["Cake", "Donut"],
    extra: true
  };
  
  let {
    size: { // put size here
      width,
      height
    },
    items: [item1, item2], // assign items here
    title = "Menu" // not present in the object (default value is used)
  } = options;
  ```
- We can also use [[Object]] Destructuring with [[Function]] parameters
  ```js
  let x = {title: 2, name : 3};
  
  function yo({title= "IDK", name= 2, age = 0}) {...}
  
  yo(x); //works
  
  //However, now yo(); would be an error, to have empty arg list we need to pass an empty object.
   
  yo({ }); //ok
  
  //Alternatively, we can set the default value of the Object itself and then we can call 
  //a function like yo without any args
  
  function hoo({title = "IDK", age= 2} = { }) {...}
  
  hoo(); //ok
  
  
  ```
-
-
-