---
categories: sldcode
layout: post
title: Testing values with Java static methods in Clojure
---

### TL;DR

- Parser functions that blow up or return falsy are excellent tools for testing values.
- Wrapping Java static methods is cumbersome and can be done with `memfn` or `#(Class/method %)` anonymous function.

### Story

I really like [Midje][midje] for testing Clojure applications as its style appeals to me.

Midje explains a test case using facts, in a very straightforward way:

```clojure
(fact "addition works"
  (+ 10 10) => 20
  (+ 20 20) => 40)
```

It also provides reasonable error messages:

```clojure
user=> (fact "addition works" (+ 10 10) => 21)

FAIL "addition works" at (form-init2621177830143524228.clj:1)
    Expected: 21
      Actual: 20
false
```

### Problem

Recently I needed to validate if a function generates a valid v4 [UUID][uuid]. This was the only use case so far,
so I did not want to pull in [clj-uuid][clj-uuid] to the project just yet and went with the usual `(str (java.util.UUID/randomUUID))`.

Testing that the UUID indeed appears where it should be can commonly be verified via:

1. Mocking the `randomUUID` call and expecting the same result. Ties the test to the implementation.
2. Trying to validate the uuid generated. Defines the expected result.

For this scenario I opted for validating the outcome. (I did **not** want to use regular expressions.)

### First attempt

Checker functions are just functions that take one argument and return a truthy or a falsy. A thrown exception is considered as a failure.

The static method [`UUID.fromString(String)`][uuid-fromstring] looks like it matches the description, so I gave it a whirl:

```clojure
user=> (fact "e79c637f-b042-4f95-918d-7b48595903c6" => java.util.UUID/fromString)

CompilerException java.lang.RuntimeException: Unable to find static field: fromString in class java.util.UUID,
compiling:(/private/var/folders/yt/fpk08__x04bghvtss5285w840000gn/T/form-init7464433185597231278.clj:1:1)
```

### Second attempt

It seems naively referring to a static method in a Java class does not work. We can always wrap it in an anonymous function though:

```clojure
user=> (fact "e79c637f-b042-4f95-918d-7b48595903c6" => #(java.util.UUID/fromString %))
true
```

### Third attempt

Anonymous functions are not very satisfying, it feels like I'm missing something here: accessing static methods as functions should not
be this awkward.

It turns out it really **is** awkward, according to [this answer on Stack Overflow][so-answer-methods]:

> Clojure functions are java methods but java methods are not clojure functions.

### Conclusion

See [TL;DR](#tldr) at the beginning :)

[uuid]: https://en.wikipedia.org/wiki/Universally_unique_identifier
[midje]: https://github.com/marick/Midje
[clj-uuid]: https://github.com/danlentz/clj-uuid
[checker-function]: https://github.com/marick/Midje/wiki/Writing-your-own-checkers
[uuid-fromstring]: https://docs.oracle.com/javase/8/docs/api/java/util/UUID.html#fromString-java.lang.String-
[so-answer-methods]: http://stackoverflow.com/a/2209786/621923
