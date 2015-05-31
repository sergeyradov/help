A package is an organization unit that can contain entities such as classes, objects and other packages. Entities that are contained in a given package belong to that package's namespace.

Note that the following examples may not work in the Scala REPL.

A package can be declared at the start of a source file:

```scala
package testplan

class TestPlan extends Simulation {
  ...
}
```

In the first line, the keyword `package` followed by the name `testplan` declares a package named `testplan`.

All following entities belong to that package.

Use of package namespaces on Flood IO will generally cause a compile error and should not be used. __Please remove any reference to packages.__