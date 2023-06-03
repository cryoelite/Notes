- ``break;``
- [[ECMAScript]] also supports Labelled Breaks
  For ex.:
  ```js
  xy: while(...) {
       while(...) {
           break xy;
         }
  }
  ```
  breaks to the labelled loop xy.
  The label can be used anywhere, only requirement is for a label to be declared before it is used in the code.
  For ex.:
  ```js
  xy: {
    break xy; //works
  }
  ```
-