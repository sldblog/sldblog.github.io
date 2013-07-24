---
categories: sldcode
layout: post
title: But soft! what build on yonder server trots?
---

I am seriously loving [Gradle](http://www.gradle.org/) after the initial 15 minutes of confused searching. No, really, if you are coming from the Java side, consider reading this page first: [Gradle Java plugin](http://www.gradle.org/docs/current/userguide/java_plugin.html), or be lost for quite some time like me. And you will probably want to run

    gradle test

first.

### What I wanted

I wanted something that used Maven's repository for fetching dependent open source libraries, while not forcing Maven's heavyweight things on me.

### What I got from Gradle

Consider the following snippet:

<script src="https://gist.github.com/4156971.js"> </script>

What did the author want?

- Oh hey, this bloke wants Java. Let's "apply" it, whatever that means.  
  *(Look, this stuff is plugin based!)*

- He wanted to use Maven central as a repository.  
  *(You can stop with Maven right there, thank you!)*

- He described his dependencies, 1 for compilation, 3 for test compilation and 1 for running his tests.  
  *(How do I configure how targets are related?...)*

- Define the source set paths, for main java, test java and test resources.  
  *(Sounds Mavenish, am I not going to be free after all?)*


### What it really is

That snippet is [i7m-common](https://github.com/sldblog/i7m-common)'s *complete* build description.

As far as I can see now, Gradle has certain perks:

- Supported by [travis-ci](https://travis-ci.org/sldblog/i7m-common/builds)!
- No boilerplate Ant `build.xml`, no init, compile, copy, test tasks, etc.
- If you're building a simple Java project with the same structure convention as Maven, you only need the first line to get a <u>full, working</u> build that can also run the tests and create JAR packages.
- If you want to use a lot of open source thingy, you can easily manage dependencies without having to commit them to your source control. (Of course, being in an IDE is an entirely different question, you will still want the libraries, or get a Gradle plugin.)
- If you want to use a different structure, just set `sourceSets` like above, and carry on.
- You don't have to worry about target relationships, because Gradle has a built-in "default" which is OK for most of the Java projects. And if someone needs more, the configuration is as easy as the `sourceSets` above.


### Closing thoughts

I am sure that it has its downsides but right now I'm pretty happy that I did not have to litter `build.xml`s into my projects.

PS. *I stole the title from [Why did the chicken cross the road? (Shakespeare)](http://www.artsforge.com/humor/chickenroad.html), which of course was based on [Romeo and Juliet](http://www.shakespeare-online.com/plays/balconyscene/butsoft.html).*
