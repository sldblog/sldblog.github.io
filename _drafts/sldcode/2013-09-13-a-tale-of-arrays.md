---
categories: sldcode
layout: post
title: A tale of arrays
---

### What

Consider this ruby code fragment for a moment:

```ruby
def date_rand(from = 0.0, to = Time.now)
  Time.at(from + rand * (to.to_f - from.to_f)).strftime("%d/%m/%Y")
end
```

What it does and how it's written is no concern at the moment. I was asked to help with a cucumber test issue
that hogged down insane amounts memory, sent the gig swapping and rendering it pretty sluggish (read: unusable).

The offending code?

The 3 lines above (actually 1... do read on).

### Why

The explanation is pretty simple, but as usual it boils down to a few little factors:

* `rand` in this case is *not* `Kernel::rand`... it's a FactoryGirl thing, which incidentally returns an ... array.
* The [`*` operator](http://www.ruby-doc.org/core-2.0/Array.html#method-i-2A) is defined on `Array`.
* An array times a really high value (eg. `1379090083.786906`) is the array duplicated as many times as the right side argument.
* Creating an array that has `1 379 090 084` one-byte elements is bad enough, imagine this with a 44 element badass sitting on the left side screaming "dupe me".

The best thing in this is that who the hell expects something like `rand` to be overwritten.

### How to fix

* Avoid returning random values in FactoryGirl defines.
* If you must, use `Kernel::rand` explicitly.
