To save you from having to specify the simulation name in the Flood UI, we automatically parse the same from your test plan based on a regular expression.

Please ensure your test plan specifies the simulation name as follows.

```scala
class WhateverYouWantToCallIt extends Simulation {
  ...
}
```