alias:: Prototype Object

- In JS, much like ``classes`` in [[C++]], an [[Object]] can inherit another Object. This is done using an inbuilt Object property known as ``[[Prototype]]`` of an Object by the spec. In Implementation, this property can be accessed using the getter/setter ``__proto__``. This is not the same as directly accessing the ``[[Prototype]]`` property. This getter/setter is historical, modern JS recommends using ``Object.getPrototypeOf(<obj>)`` and ``Object.setPrototypeOf(<obj>, <prototype object>)`` instead. We can also use ``Object.create(<prototype object>, <descriptor object>)`` to create an Object with a prototype set.
  
  This property can have either the value of [[null]] or another Object.  
  ![image.png](../assets/image_1686622520624_0.png)
  
  If set, it acts like inheritance. So if we access a property and it is not present in the ``child`` Object, it is looked for in the ``parent`` Object. And if it is in both then the child Object's property has the parent's property [[Shadowed]].
  
  For ex.:
  ```js
  let x= {
    run: true,
    cow: "noo"
  };
  
  let y= {
    cow: "nyaa"
  };
  y.__proto__ = x;
  console.log(y.run); //prints true
  console.log(y.cow); //prints nyaa
  console.log(y.__proto__ == x); //true
  ```
  
  We can directly use ``__proto__`` in an Object too.
  ```js
  let x ={};
  let y = [
   __proto__: x, //works
  };
  ```
  but all these ways of getting/setting/initializing the `` [[Prototype]]`` have their modern equivalents
  ```js
  let y ={}
  let x = Object.create(y, {
   yo: {value: 2}
  });
  console.log(Object.getPrototypeOf(x) == y); //true
  let z ={};
  Object.setPrototypeOf(x,z);
  console.log(Object.getPrototypeOf(x) == z); //true
  ```
  
  However, it is unrecommended to get/set ``[[Prototype]]`` often, it is acceptable for one time but not anymore as each operation is slow. The JS engine has many optimizations on Objects and they break if we modify``[[Prototype]]`` hence leading to performance loss.
- There can only ever be a single ``[[Prototype]]`` of an Object, so this is akin to ``Single Inheritance`` in other languages, we can have chain/hierarchy, and modify the parent Object at any time, but there can only be 1 parent to any Object.
- The whole ``prototypal chain`` is traversed to look for a property, that means even if we have longer inheritance chains, it will work similarly. 
  The only limitations are, if there's a cycle in this chain then that causes an error and that the value of ``[[Prototype]]`` must always be either null or another Object.
  
  For ex.:
  ```js
  let x= {
    run: true,
    cow: "noo"
  };
  
  let y= {
    cow: "nyaa"
  };
  y.__proto__ = x;
  console.log(y.run); //prints true
  console.log(y.cow); //prints nyaa
  ```
  
  This only works for reading, for writing, the given [[Object]] is the one used. The only exception is when we set a value using an accessor [[Function]] like so
  ```js
  let x= {
    a:2,
    b: 5,
   get ap() {return this.a;},
    set ap(value) { this.a = value; },
    bp() {return this.b;},
  };
  
  let y ={
   __proto__: x,
  };
  
  y.a=4; 
  console.log(x.a); //prints 2
  
  console.log(y.a); //prints 4
  console.log(y.bp()); //prints 5
  ```
  Here [[this]] is ``y`` and not ``x``, this is because ``this`` gets the Object that called the method, not the ``this`` that refers to the same Object i.e. ``x``. Here, even though ``this`` is y, when we get b through bb(), it gets b from x as it follows the inheritance chain for reading b (remember, ``this`` is just an Object so it's not doing anything special here).
- In [[Object]], ``<obj>.hasOwnProperty(<prop name>)`` returns true if a property belongs to the Object and is not inherited.
- ``Object.prototype`` [[Object]]
  
  Every Object in JS inherits this special Object, this is the Object that defines methods like ``.hasOwnProperty(...)`` on Objects. All its properties have their attributes ``enumerable`` as false so they don't appear in any loops, similarly ``Object.keys`` and other methods also ignore this Object's properties. 
  
  So
  ![image.png](../assets/image_1686686755458_0.png)
  Assume rabbit and animal are any object and rabbit inherits animal, then as we can see, even animal implicitly inherits from this special Object.
  
  We can even check it,
  ```js
  let obj = {};
  
  console.log(obj.__proto__ === Object.prototype); // true
  ```
- [[Function]]'s ``<func>.prototype``:
  Every Function has a ``.prototype`` property, that assigns an Object to the ``[[Prototype]]`` of the newly created Object when using [[new]]. 
  For ex.:
  ```js
  let animal = {
    eats: true
  };
  
  function Rabbit(name) {
    this.name = name;
    console.log(this.eats); //works if the .prototype is set before this function is ran
  }
  
  Rabbit.prototype = animal;
  
  let rabbit = new Rabbit("White Rabbit"); 
  
  console.log( rabbit.eats ); //prints true
  ```
  
  The ``<func>.prototype`` is a property and not the ``[[Prototype]]`` itself, 
  ![image.png](../assets/image_1686787038439_0.png)
  We can also access ``animal``'s properties in the function ``Rabbit(...)`` as well, as [[this]] Object inherits it first then passes this Object back to the caller. This is to say, ``.prototype`` does nothing but sets the ``[[Prototype]]`` of the ``this`` Object and the rest works as expected.
  
  * If we change the ``.prototype`` then it changes the ``[[Prototype]]`` for the Object's created after that assignment, but not for Object's created before it. 
  
  * ``.prototype`` is only passed when the function is called with ``new <func>(...)``. However, it is always set to some value so it always exists.
  
  * Default ``<func>.prototype``:
  Every function has a  default ``.prototype`` set, this is set to an Object that has a single property named ``constructor`` which points back to the function. 
  For ex.:
  ```js
  function yo() {}
  // yo.prototype = { constructor: yo };  implicitly
  
  console.log( yo.prototype.constructor == yo); //prints true
  
  let x = new Yo();
  console.log( x..constructor == yo); //also true because the prototype is always inherited with new <func>(...)
  
  //so we can do this too
  let y= new x.constructor(); //works
  ```
  We can work with this ``constructor`` property however we want and JS has no check against it, so we can set it to other functions, have it in our custom Object and then use that Object as ``[[Prototype]]`` etc.
- As we know, **all** [[Object]]s inherit from ``Object.prototype``, this is true for [[Array]], [[Function]], [[Number]] etc.
  
  ![image.png](../assets/image_1686712608418_0.png)
  
  These Object's also have their own `.prototype` Objects, called ``native prototypes`` which then define helper properties/methods, and when we create their ``<object>``'s using either literal syntax or [[new]] syntax (recall that they are the same thing, ``new Object()`` is always called), the ``<object>``'s actually inherit ``native prototypes`` which all inherit the ``Object.prototype``
  Like so
  ![image.png](../assets/image_1686789747813_0.png)
  
  Basically, all data types ([[String]], [[Number]] and [[Boolean]]) except [[null]] and [[undefined]] have their helper functions/properties defined in special Object's, these are known as ``wrapper objects`` and whilst the value theirselves are primitives, the ``new String()``, ``new Number()``, etc. use these Object wrappers aka ``native prototypes`` which have their ``.prototype`` set to these Objects with the helper functions.
  null and undefined are special as they have no wrappers, hence they have no properties either.
-
- We can modify the native prototypes to extend [[String]]/ [[Number]]/[[Boolean]]/[[Array]] etc.
  Like so
  ```js
  String.prototype.yo = function() {
   console.log(this);
  };
  
  "hayo".yo(); //prints "hayo"
  ```
  It's unrecommended to modify native prototypes as it may be overwritten by some external script if we use one, and secondly other developers may not be aware if a given function is inbuilt or custom making it harder to maintain the code. 
  However, it is acceptable to modify the native prototype to [[Polyfill]] some functionality, i.e., the property exists in some other versions so to make sure your own script has it we can do so.
- We can borrow prototypal methods too, 
  For ex.:
  ```js
  let x = {
   0: "Yo",
   1: "ayo",
   length: 2,
  };
  x.prototype = Array.prototype.join;
  console.log(x.join(" ")); //prints Yo ayo
  ```
  We can make ``x`` an [[Array]] or some other Object Wrapper too given we set it's ``[[Prototype]]`` to the appropriate native prototype.
- [[null]] ``[[Prototype]]``
  As we know
  ```js
  let x = {};
  x.__proto__  =2 ; //ignored
  console.log(x.__proto__); //prints [object object]
  ```
  This is because ``__proto__`` is an accessor property to get/set the ``[[Prototype]]`` and the actual ``[[Prototype]]`` comes from ``Object.prototype`` which is implicitly set for all Objects. That is, ``x.__proto__`` above is ignored because ``x``'s ``[[Prototype]]`` is already set and now ``x.__proto__`` is accessing the ``Object.prototype``'s accessor property ``__proto__`` so we are essentially calling the accessor property in ``Object.prototype``method, which looks like so ``x.__proto__.__proto__=2`` .
  To circumvent this, we can use a trick
  ```js
  let x = {__proto__=null};
  //or let x = Object.create(null);
  x.__proto__=2
  console.log(x.__proto__); //prints 2
  
  //or
  
  let z ={};
  Object.setPrototypeOf(z, null);
  z.__proto__= 2;
  console.log(z.__proto__); //works
  
  ```
  By setting the property explicitly to null already, JS doesn't implicitly set the ``__proto__`` to ``Object.prototype`` which has an accessor property named ``.__proto__``. The ``Object.setPrototypeOf(...)`` is also smart enough to correctly do it.
-