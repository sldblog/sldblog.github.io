---
categories: sldcode
layout: post
title: TypeError message change
---

Just as an interesting fact:

```ruby
[1, 2, 3].first("two")
```

Will give the following responses respective to Ruby version:

- [Ruby 1.x](http://www.ruby-doc.org/core-1.9.3/TypeError.html): `TypeError: can't convert String into Integer`
- [Ruby 2.x](http://www.ruby-doc.org/core-2.1.2/TypeError.html): `TypeError: no implicit conversion of String into Integer`
