---
categories: sldcode
layout: post
title: Time.mktime/iso8601 off by millisecond
---

`Time` class in Ruby has the following interesting behaviour when using `iso8601` with fraction digits:

```ruby
irb(main):003:0> Time.mktime(2013, 11, 2, 21, 27, 16.424).iso8601(3)
=> "2013-11-02T21:27:16.423+00:00"
```

The result is off by one millisecond.

## Workaround

This can be worked around by using either a non-zero additional millisecond digit in mktime, *or*
using the `round` function before `iso8601`:

```ruby
irb(main):004:0> Time.mktime(2013, 11, 2, 21, 27, 16.4241).iso8601(3)
=> "2013-11-02T21:27:16.424+00:00"
irb(main):005:0> Time.mktime(2013, 11, 2, 21, 27, 16.424).round(3).iso8601(3)
=> "2013-11-02T21:27:16.424+00:00"
```

This behaves the same in both Ruby 1.9.3 and 2.0.0.

## Causes

TODO
