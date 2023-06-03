- It is recommended to do ``Behavior Driven Development`` (BDD) which means we should write Tests, Documentation and Examples for all functions/features.
- For ex.:
  ```js
  describe("pow", function() {
  
    it("raises to n-th power", function() {
      assert.equal(pow(2, 3), 8);
    });
  
  });
  ```
  Using ``Mocha`` for ``describe`` and ``it``, ``Chai`` for ``assert.equal`` and ``Sinon`` to inject into functions.