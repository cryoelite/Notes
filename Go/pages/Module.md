- Every go project is a single module, a module is a collection of single or more go [[Package]]s.
  For ex:
  A sample [[Go Project File Structure]] 
  ```
  project-root-directory/
    go.mod
    modname.go
    modname_test.go
  
  ```
  This is the simplest Go Module, a project with just a single package, ``main``. And this module is called ``modname``.
  
  Modules are Go's way of dependency management. 
  Each Go [[Package]] is just a module, and it may have many submodules in it.
- ``go.mod``
  This file defines all the modules and their location in a project directory. 
  It's the equivalent of a ``pub.spec`` file in a Dart Project.
-
-
-
-
-
-
-
-