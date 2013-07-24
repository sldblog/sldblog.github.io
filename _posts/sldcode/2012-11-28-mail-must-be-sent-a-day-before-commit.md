---
categories: sldcode
layout: post
title: Mail must be sent a day before commit
---

A friend of mine has just received a mail, that contains instructions on the project his team is working on (excerpt):

- Every change to XML/XSD/XSL must be reviewed by the (censored) review board group.
- \[...\] A change in XML needs to be announced to the (censored) mail distribution list. This distribution list does not exist in the exchange server yet so you can send me the email and I can forward it for the moment.
  - Ideally a mail should be sent at the start of the work with the expected commit date.
  - A mail must be sent a day before commit.

I'm sorry. I understand the need of control, but this is a seriously fucked up way to do it. What I think is:

- This is a distributed project with a lot of teams depending on a central XML/XSD.
- There are no branches, it is a single track development (in fact I know that it is hosted in SVN despite being such a huge thing).
- There are no tests, therefore no continuous integration.
- Instead of fixing things, more bureaucratic weight is placed on the developers to create an illusion of control.
- There isn't a mechanism to easily and effectively distribute change notices through teams.
- The authors are basically stating that developers do not have the necessary knowledge to propose changes in the XMLs.
- This leads to the conclusion of no documentation, from which the developers would learn.

### So what

Developers are best at hacking away at code and providing solutions to the requirements. It's their job! They love their job! (Some even grow facial hair because they would rather hack at code than play with sharp blades.)

This kind of control will only throw them off balance, making it impractical to even suggest changes that may make the whole project easier.

Practically the mail makes the XML/XSD/XSL in the system a fixed fact ("Don't touch it, you!"). No one but the bravest will try to fiddle with it, therefore solutions will pop up that go around the problem. They may not be big things -- only there's a possibility that out of two solutions, the better will be dropped.

There still won't be tests -- so we now have hacks in the code, but the management is confident because nothing changed in the critical area.

### Commit control?

Tasks that involve adding a single optional attribute that would take anyone 25 minutes will suddenly be days. First the approval, then the announcement, then the "will commit" ping, then 24 hours later, the commit. Changes will be batched, it will be unclear if they're logically grouped or it just happened that way.

Sending a mail that the change will be committed 24 hours later creates a tension that practically will manifest as: done &rarr; send mail &rarr; commit tomorrow. Bam, we have a 24h delay. Not good for anyone.

### Conclusion

I may overreact to this; I can easily imagine a situation that such rigorous verification is necessary. Only that it should be supported via the build and continuous integration, not via pre-commit emails, not via restrictions on design feedback from developers. This is too much of a cost, and ultimately the product has a good chance to be the loser of this.
