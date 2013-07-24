---
categories: sldcode
layout: post
title: Create toolkits, not drones
---

### Frustration

I tend to claw my face off when I need to do something tedious. When I'm presented with a task that would take half minute with an ideal solution, but takes 30 minutes with crosschecking textual data. I hate to do irrelevant stuff.

The example I will be telling was how we created a toolkit for our project.


### Workflow

Our workflow was a bit of a myth. It had to be identified; not everyone followed the same patterns, but overall, the following workflow became visible:

1. create initial dataset
2. deploy and/or run our application
3. analyze generated data
4. optional: remove generated data
5. optional: goto step 2
6. remove initial dataset

Sometimes we would keep deploying and testing with the same initial dataset, cleaning only the generated data.

I'll now describe each step in detail, in the order they became a problem. We were promised there'd be a tool that takes care of the initial dataset problem, so I won't be talking about steps 1 and 6.

#### \#4: Remove generated data

Identify the generated data in the database and remove only the data that was generated. The database is essentially a tree-like structure with "Key=Id" values in leaves (not XML). Delete only given keys with an id regexp for each key that matches only generated leaves.

It was fairly obvious we have to implement a script that deletes these leaves, or we can't test without resetting the whole database. Initial implementation was hard to configure and slow: it started a new session for each operation, which is very slow for this database. This solution was used for lack of a better version, and neglected to be updated, since "we don't have time for that".

This was of course utter bullshit. We were wasting tens of minutes on a slow script for days, instead of investing 1 day for 1 programmer.  
Sure, I had to dig into the API of the database for how to do everything in one session and one transaction, but it's now common knowledge (it was also used in the initial dataset scripts to make them faster).  
Sure, I had to invent a whole new configuration scheme, and write a couple of ruby classes.

**The result**: it was half a day, it now runs under seconds, and adding a new key with a new id regexp is about 3 lines.

#### \#5: Goto step 2

As easy as this sounds, we had to use a GUI tool (external product that used our service) to call our application. Instead of simple SSH, we had to use graphical sessions and using products that integrated with our solution. It was *slow*.

CLI client that would call our service was dropped first, but put back when the GUI tool was late, as a necessary evil.

**The result**: everything can be automated from scripts, without using graphical terminals.

#### \#3: Analyze generated data

I wasn't aware of this problem until I had to do some testing much later in the project. I was horrified by the amount of monkey-work that needed to be done to test the result. And *no one was crying for help from the test team*!

This was on Friday. I was so frustrated that I came in Saturday, and created a visual representation of the result using graphs. I could reuse most of the ruby classes from step 4. Graphviz is awesome, and easy to generate input for it.

**The result**: validating a result is now about 1,5 minutes. Product managers from across the globe actually want the solution to be productified because it actually represents the data in a friendly, colourful way.

#### \#2: Deploy and/or run our application

We actually made this part of the normal development process, but there was a time when the only way to deploy something was to build an official package, scp it to the test server, install and start manually.

Being too lazy, we made a build target that created the necessary JARs instead of a full build, copied them to the server configured, and restarted the service remotely, if needed.

**The result**: deploying an application is a double-click from Eclipse, and is up-and-running in about 40 seconds if a full restart is needed.


### Conclusion

- Investing in toolkit scripts resulted in a far more quicker, "agile" workflow.
- The scripts reduced our outside dependencies and enabled automation to an extreme level.
- It was not part our normal planning process to evaluate potential toolkit components, but since everyone saw how it worked, now it is.

Overall, doing something not visible to the stakeholder is always worth the extra few man-hours, if the overall process will be faster and easier to use, with better quality. The problem is, most companies and customers only want what is immediate value to them, ignoring everything else. Our application now has the lowest number of bugs ever and didn't miss the deadline.
