---
categories: sldcode
layout: post
title: jruby-18mode and 1.9 hash syntax
---

I have this pet sandbox project named [kandic](https://github.com/sldblog/kandic) where I try myself at Ruby BDD while creating something worthwhile. I hooked up with [travis](https://travis-ci.org/sldblog/kandic) and [codeclimate](https://codeclimate.com/github/sldblog/kandic) for good measure too. Travis is set up to test against 4 different RVMs: `1.8.7`, `1.9.3`, `jruby-18mode`, `jruby-19mode`.

And here's the funny thing: [as it turns out](https://travis-ci.org/sldblog/kandic/builds/3354134), the 1.9 hash syntax that was in the code that time properly creates parse errors on `1.8.7`, but `jruby-18mode` can parse it.

I had the same problem with `minitest` dependency later, the `jruby-18mode` was happily crunching away.

Just a few things to watch out for if the target is 1.8, I guess.
