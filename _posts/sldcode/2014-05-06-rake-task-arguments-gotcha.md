---
categories: sldcode
layout: post
title: rake task arguments gotcha
---

## Parameterised tasks

`rake` tasks are brilliant, useful and easy to write. Rake contains a feature pass down arguments to tasks within square brackets:

```
$ rake 'healthcheck[cluster1]'
```

The syntax to define this is by naming the arguments as symbols after the task name and provide a block with a task and an arguments hash as parameters:

```ruby
task :healthcheck, :scope do |task, args|
  result = Healthcheck.run(scope: args[:scope])
  abort unless result.healthy?
end
```


## Gotcha

The slight problem with this syntax is there are no explicit checks whether the arguments are defined. Value checking and providing
default values is left to the user: `rake healthcheck` will happily run with an empty `args` hash.

One way would be to use [`Hash#fetch`](http://www.ruby-doc.org/core-2.1.1/Hash.html#method-i-fetch) to provide a default value:

```ruby
result = Healthcheck.run(scope: args.fetch(:scope, :all))
```

However this **won't** work, and will call `Healthcheck.run(scope: nil)`.


## Why

Arguments passed in into tasks are instances of [`Rake::TaskArguments`](https://github.com/jimweirich/rake/blob/master/lib/rake/task_arguments.rb) instead of `Hash`.

`TaskArguments` works the following way:

- Does not define `fetch` or other hash-only methods, but includes `Enumerable`.
- Defines a custom `method_missing` that will act as a method lookup for the symbols for the arguments.  
  Because of this, `args.fetch` is equivalent to `args[:fetch]`
- It defines a `to_hash` method that publishes the argument hash.

Usually and ideally the number of arguments to a rake task is not high, so this parameter handling should not be a problem anyway.
