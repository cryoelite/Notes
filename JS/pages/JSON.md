alias:: JavaScript Object Notation

- General format to represent values and objects, as described in [RFC4627](https://datatracker.ietf.org/doc/html/rfc4627)
- The JSON [[Object]] helps parse and create string from JS Objects. 
  ``JSON.parse(<str>, <optional reviver function (key,value)>)`` parses a str and returns an Object 
  ``JSON.stringify(obj, <optional replacer function (key,value)/array for selecting properties>, <optional int indent space count>)`` Creates a string from an Object. This String follows JSON formatting.
- The ``.stringify(...)`` method accepts an optional replacer array, only properties given in this array are taken from the object. Or if a function then maps all keys to new values returned by it in the resulting string.
  For ex.:
  ```js
  let meetup = {
    title: "Conference",
    participants: [{name: "John"}, {name: "Alice"}],
  };
  
  
  console.log( JSON.stringify(meetup, ['title', 'participants']) );
  // {"title":"Conference","participants":[{},{}]}
  ```
- The ``.stringify()`` first checks if the [[Object]] has a ``toJSON()`` [[Function]] in it. If it does, then it calls it and it's result is the return of the function instead.
  ```js
  let x= {
   a:2,
   toJSON() {
    return this.a;
    }
  };
  
  console.log(JSON.stringify(x)); //prints 2
  ```
- Similar to ``.stringify(...)``, ``.parse(...)`` accepts a method. Each key and value picked is passed to it and the value returned by the function is assigned to the key in the resulting object.
-