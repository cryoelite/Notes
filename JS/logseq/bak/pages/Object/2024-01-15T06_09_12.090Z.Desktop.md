- The signature ``object`` type.
  It's a non-primitive type that can contain collections of data with multiple entities, think of them as "Map" or "Dictionary" from other languages but with the extensibility of "Classes". They are used everywhere in JS, and almost everything in JS is an Object or uses Object.
  For ex.:
  ```js
  let obj= new Object();  //Object Ctor Syntax 
  let obj2 = { }; //same thing, Object Literal Syntax, implicitly calls new Object()
  ```
- Objects store ``Key:Value`` pairs, each pair is called a ``Property`` and the key is called the ``Property Name`` whilst the ``Value`` is just called the ``Value``.
  
  For ex.:
  ```js
  let obj = {
   name: "YOo",
   age: 200,
   "yoo yoo": 20, //This is a multi-word property name
  }
  ```
  Objects in JS are hash tables, so the Time Complexity of accessing a key is $$\Theta\text{(1)}$$ and O(n).
  
  To create/update a property
  ```js
  obj.name= 2;
  //or  
  obj["yoo yoo"] ="yo";
  ```
  
  To retrieve
  ```js
  console.log(obj.name); //works
  //or  
  console.log( obj["yoo yoo"] );
  ```
  
  To delete
  ```js
  delete obj.name; 
  //or
  delete obj["name"]; 
  ```
  Multi-word Properties can only be accessed with [...] and not the Dot [[Operator]].
-
- [...] access also supports variables inside them.
  For ex.:
  ```js
  let x= "yo";
  let y= {...};
  y[x]; //ok 
  ```
- Computed Properties
  We can use variables as property names too.
  For ex.:
  
  ```js
  let x= "yo";
  let y= {
     [x]: 2,
     [x+"X"]: 34,
  };
  y[x]; //works, returns 2
  y["yo"]; //also works
  y.yo; //works!
  y.yoX; //same
  
  ```
- To check if a key exists in an Object we can use the ``in`` [[Operator]]
  ``if "x" in myobj``
  We can also use ``if myobj["x"]===undefined`` but this also returns true if "x" does exist and stores a value of [[undefined]] in it.
- Property Value Shorthand
  If a key name and variable holding its value has the same name, we can omit the value and it will pick it up from the variable.
  For ex.:
  ```js
  let name = "yo";
  let y= {
     name, //that's it.
  }
  y.name; //works and returns "yo"
  ```
- Keys / Property Names can be either strings or [[Symbol]]s only, the other type are converted to strings. There is no restriction to a property's name as well. We don't need to use ``" "`` for strings for property names, it is automatically inserted.
  For ex.:
  ```js
  let x= { 
    return: 2, //ok
    "yo": 23, //  using string syntax
    0: 2, //0 is of type Number, so it is converted to "0"
    b() {
      this.y=2; //works too, sets x to 2 for ``this`` Object, which can be x.
    }
  }
  x.return; //works, returns 2
  x["0"]; //returns 2 
  console.log(x.y); //prints undefined
  x.b();
  console.log(x.y); //prints 2
  ```
  There's a widely accepted convention that property names that start with "_" are ``hidden`` properties and hence mustn't be accessed by outside code. JS doesn't enforce it by default.
-
- Methods
  * [Object.keys(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) – returns an [[Array]] of keys (doesn't include [[Symbol]] properties or [[Prototype Object]] properties).
  * [Object.values(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/values) – returns an array of values.
  * [Object.entries(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) – returns an array of `[key, value]` pairs.
  ``Object.getOwnPropertySymbols(obj)`` to get array of Symbol Keys.
  ``Reflect.ownKeys(obj)`` to get array of all keys, including Symbol properties.
-
- for..in [[Loop]] can be used with [[Object]], and also with the various methods of Object that return arrays.
  This loop doesn't iterate over Symbol Keys.
  This loop even iterates over properties of the [[Prototype Object]].
- Integer Properties and Ordering
  When we loop over keys of an Object, they appear in the order they were created unless they are Integer Properties.
  
  Integer properties are property names that are integers or strings that are directly and fully convertible to integers. Integer properties are sorted automatically.
  For ex.:
  ```js
  let x= {
    "24": "yp",
    "2": "aa"
  }
  let y= {
    "+24": "yp",  //Whilst it can be directly converted to number 24, it is not fully convertible as + is lost
    "abc": "aa"
  }
  
  for (let key in x) {...} //key is 2 then 24 
  for (let key in y) {...} //key is +24 then abc
  ```
- Variables storing [[Primitives]] store their value directly, i.e., the variable of a primitive is a memory location holding the value directly. However [[Object]]s are always stored by reference.
  For ex.:
  
  ```js
  let user = {
    name: "x"
  }
  ```
  Here user actually stores the address of the Object.
  ![image.png](../assets/image_1685705623118_0.png){:height 272, :width 371}
  
  This is also to say, if we copy an object by assignment operator, we actually copy the address and not the actual object and hence we copy by reference.
  ```js
  let x = {...}
  let y= x; 
  //any change on y or x will be visible to both
  ```
- Comparison
  To compare objects we can use the simple ``==`` Operator. It compares the address pointed by the variable.
  ```js
  let x= { };
  let y= { };
  x==y; //false
  y= x;
  x==y; //true
  ```
- To copy an Object,
  Shallow Copy: Copy using assignment operator
  Deep Copy: Iterate over keys and assign each primitive to the other Object. 
  Or
  use ``Object.assign(dest, ...sources)``
  This method returns the dest Object back as well. This method also copies over [[Symbol]] properties.
  However it is still only a single level copy, if there are multiple levels to an Object, i.e., it has nested Objects then it will shallow copy the further levels. 
  
  To deep copy all levels of an Object, we can iterate over keys and check if a key is an Object and if it is we recursively go in it and copy all the primitives
  Or
  use ``let dest =structuredClone(source)`` which does the same.
  ``structuredClone`` also solves circular reference correctly.
  That is,
  ```js
  let x= { };
  x.me= x;
  
  let y= structuredClone(x);
  y===y.me; //returns true
  ```
  However, it can't copy functions defined in an object so either custom loop or using an external library is required.
  
  Anoher way of copying an Object is 
  ```js
  let x={};
  let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(x));
  ```
  This works because ``Object.defineProperties(...)`` defines properties with their descriptors on an empty object and returns it, and ``Object.getOwnPropertyDescriptors(...)`` gets all properties with their descriptors. Even copies other Objects such as [[Function]]s.
  
  Similarly, there's yet another way of cloning an Object with everything, including its [[Prototype Object]]
  ```js
  let obj = {};
  let clone = Object.create(
    Object.getPrototypeOf(obj), Object.getOwnPropertyDescriptors(obj)
  );
  ```
  This uses ``Object.create(<prototype object>, <descriptors>)`` to create an object and even sets the same ``[[Prototype]]`` as the original Object.
-
- Methods
  Properties of Objects that hold [[Function]]s.
  For ex.:
  ```js
  let x= { 
    bo: function() {...}, 
    bow() {...},    //method shorthand
  
  };
  x.yo= function() {...};
  
  x.yo(); //works
  ```
  These are the 3 ways to create a method in an Object.
  
  To access the properties of the object inside the function property, we can use the [[this]] keyword.
-
-
- Optional Chaining [[Operator]]
  If a property is [[undefined]] for an object, using this operator we can get undefined instead of an error and if it's not undefined, we can get that property's value instead. 
  ``?.``
  For ex.:
  ```js
  let x= {...};
  console.log(x?.name?.yo); 
  ```
  If x.name exists then get it then if name.yo exists get that. If x.name doesn't exist immediately return undefined, this is also a [[Short-circuit Evaluation]] . This also avoids an error if a property that doesn't exist is accessed using the dot operator.
  
  For methods, we can use the ``?.()``variant.
  That is,
  ```js
  let y= {};
  console.log(y?.yo()); //error
  console.log(y.yo?.()); //ok, returns undefined
  ```
  Similarly, if we are using ``[]`` to access properties, we can use the ``?.[]`` variant.
  ``y?.["abc"]`` works.
  
  And this also Op also works with delete,
  ``delete y?.name;``
- [[Object]] Wrapper:
  Primitive types have no properties/methods, they only have values as they are meant to be fast and lightweight. This is why JS provides Object wrappers for almost all primitive types, as these Objects provide the utility methods and also store the type and its value.
  A key principle of Object wrappers is to never return the Object wrapper itself, always the primitive value associated with it. 
  For ex.:
  ```js
  let x = String("yo") //is an Object Wrapper String(), but it returns the primitive type string.
  ```
  
  We can get the Object wrapper back from an Object Wrapper using [[new]], however it is highly unrecommended.
  ```js
  let x = new Number(2) //returns an Object
  ```
  This is only valid as it is meant for internal use only.
  
  As for properties and methods on primitive values, they come from the Object Wrappers, but JS simply inserts the methods/properties automatically.
  ```js
  let x = 2;
  x.toString();
  ```
  Here .toString() is added automatically by JS later.
  For [[Number]] specifically, if we want to call a method/property on a direct value, we have to use 2 dot [[Operator]] ``..``, this is to avoid ambiguity we'd have with a single dot which also stands for decimal.
  For ex.:
  ```js
  123..toString(); //ok
  //or
  (123).toString();
  ```
-
- [[null]] and [[undefined]] have no Object Wrappers associated with them.
- ``Object.is(a,b)`` is the same as ``a===b`` for [[Comparison]]. However it also handles 2 edge cases, if a and b are NaN, it returns true as [[NaN]] returns false by direct comparison. Secondly it returns false if a is -0 and b is 0 (or the other way around).
- Object properties aren't simply just ``Key:Value`` pairs, they also have other sub-properties/attributes, known as ``flags``. There's 3 of them and these are all boolean ``Key:Value`` pairs,
  
  * ``writable``: if the property's value is writable(true)/read-only(false)
  * ``enumerable``: if the property is enumerated in loops/Object.keys and the like (true), or not (false)
  * ``configurable``: If true, allows these attributes to be modifiable and the property to be deletable. Regardless of this attribute, the value itself is mutable if it is writable. This is a one-way road, an attribute set to false configurable can't be reverted back. However we can modify writable attribute if it is true and set it to false if configurable is false.
  All 3 are true for properties by default. 
  
  In [[Old Mode]], flag violations (such as writing value when ``writable`` is )are silently ignored and the changes not applies. In ``Strict Mode``, flag violations result in an [[Error]] and the changes aren't applied. 
  
  To get these attributes we use ``Object.getOwnPropertyDescriptor(<obj>, <prop name>)``
  For ex.:
  ```js
  let x= {
   a:2,
   b() {
    }
  };
  let descriptor = Object.getOwnPropertyDescriptor(x, "a"); //descriptor is a normal Object representing //the attributes
  console.log(descriptor)
  //prints { value: 2, writable: true, enumerable: true, configurable: true }
  
  console.log(Object.getOwnPropertyDescriptor(x, "b")); 
  //prints { value: [Function: b], writable: true, enumerable: true, configurable: true }
  ```
  Similarly, we have ``Object.getOwnPropertyDescriptors(<obj>)`` which returns all the properties with their descriptors from an Object.
  
  We can set/update these attributes using ``Object.defineProperty(<obj>,<prop name>, <descriptor obj>)``
  For ex.:
  ```js
  let x= {
   a:2,
  };
  Object.defineProperty(x, "b", { value: 4, writable: false });
  console.log(Object.getOwnPropertyDescriptor(x, "b"));
  //prints { value: 4, writable: false, enumerable: false, configurable: false}
  ```
  If the property doesn't exist, it is created. Then for it's attributes, if an attribute is supplied it is set to the given value, otherwise it is set to false. 
  
  Similarly, we have ``Object.defineProperties(<obj>, <Object like so {
  <prop 1 name>: <descriptor1>, 
  <prop 2 name>: <descriptor2>, 
  ...
  }>)``
  to set multiple properties with their descriptors at once. Attributes not set are false for any property here as well.``Object.defineProperties(...)`` also returns the modified Object.
  
  This is why combining ``Object.defineProperties(...)``and ``Object.getOwnPropertyDescriptors(...)`` allows for another type of copy. As properties with their attributes are copied over, even properties that aren't ``enumerable`` are copied over this way.
- Sealing an Object:
  
  * [Object.preventExtensions(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/preventExtensions)
  : Forbids the addition of new properties to the object.
  * [Object.seal(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/seal)
  : Forbids adding/removing of properties. Sets `configurable: false` for all existing properties.
  * [Object.freeze(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)
  : Forbids adding/removing/changing of properties. Sets `configurable: false, writable: false` for all existing properties.
  
  And also there are tests for them:
   * [Object.isExtensible(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/isExtensible)
  : Returns `false` if adding properties is forbidden, otherwise `true`.
  * [Object.isSealed(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/isSealed)
  : Returns `true` if adding/removing properties is forbidden, and all existing properties have `configurable: false`.
  * [Object.isFrozen(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/isFrozen)
  : Returns `true` if adding/removing/changing properties is forbidden, and all current properties are `configurable: false, writable: false`.
- Getter/Setter [[Function]]s (Methods):
  Implemented using ``Accessor Properties`` which have syntax almost like a function with special keyword ``get`` and ``set`` which get/set value of/for a property.
  For ex.:
  ```js
  let x = {
   a: 2,
   b: 3,
   get ab() {
    return a+b;
  }
  set ab(value) {
   this.a= value/2;
   this.b= value/2;
  }
   
  };
  console.log(x.ab); //prints 5
  x.ab(4); //sets a and b to 2.
  ```
  Properties like a and b are called ``data properties`` whilst methods like these are called ``accessor properties``. There's only these 2, so if any property uses get/set it is an ``Accessor Property``.
  
  As we saw above, ``ab(...)`` isn't a normal method/function as there's 2 of it. So the accessor property is ``ab``.
  
  G/Setters can't have the same name as any other property/method, just like normal methods/properties.
- Accessor properties have a different Object for descriptors, it has
  `get` – a function without arguments, that works when a property is read,
  `set` – a function with one argument, that is called when the property is set,
  `enumerable`– same as for data properties,
  `configurable` – same as for data properties.
- There is a special property [[__proto__]] which can't be set for an Object to a non-Object value, that is ``x.__proto__= 2; `` will work but retrieving ``x.__proto__`` will return ``[object] [object]``.
  This is because, [[Prototype Object]] is a property for any Object that defines its ``Prototypal Inheritance``.
- ``Object.toString()``
  This is quite a powerful method as it can be extracted and then used to identify Object type.
  For ex.:
  ```js
  let s = Object.prototype.toString;
  
  console.log( s.call(123) ); // [object Number]
  console.log( s.call(null) ); // [object Null]
  console.log( s.call(alert) ); // [object Function]
  ```
  It examines the [[this]] Object and returns a string.
  
  If our Object has a special [[Symbol]] property [Symbol.toStringTag] set with a value of a string, then toString() gets that value instead.
  For ex.:
  ```js
  let x = {
   [Symbol.toStringTag]: "Yo",
  };
  console.log({}.toString(x)); //prints Yo
  console.log(window[Symbol.toStringTag]); //prints window
  ```