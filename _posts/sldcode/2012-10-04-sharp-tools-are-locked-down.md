---
categories: sldcode
layout: post
title: Sharp tools are locked down ...
---

... because no one likes the children in pieces.

[Powermock](http://code.google.com/p/powermock/) can be useful in trying to handle legacy code, but I think our team made a big mistake by promoting it in an ongoing fresh development.

It allows us to just ignore certain obvious design problems. If we cannot test a code without major headaches, then we have an architectural problem that needs dire attention. Solving the test won't make the problem go away, and we'll have to face it again, possibly with smaller timeframe and a much bigger mess.

To reiterate, doing this in legacy code is fine by me. Doing it in code that we can actually shape and refactor easily -- *and is not final or designed in a way that usually prevents testability* -- is a major risk and the question *&quot;How can we fix this at the source?&quot;* is a way better solution than relying on powerful tools to do it for us.
