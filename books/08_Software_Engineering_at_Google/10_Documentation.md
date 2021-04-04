### 10. Documentation

- Documentation scales: It is written once but read many times over
- Documentation helps on-board new team members
- > If you have to explain something to someone more than once, it usually makes sense to document that process
- > Like code, documents should also have owners. Documents without owners become stale and difficult to maintain
- Google tries to make it as easy as possible to write documentation: It can be edited just like code and the same tools are used for reviews
- Think about your audience when writing documentation
- Two types of document readers
	1. *Seekers*: Know what they are looking for
	2. *Stumblers*: Not sure which document they need
- Make sure each document only has one topic
- Code comments
	- There's two types of code comments
		1. API comments (no implementation details)
		2. Implementation comments (can assume more domain knowledge)
	- Be aware of which one you are writing:
	- Code files at Google always start with a comment describing the file. This is useful for stumblers. Individual classes and methods are then also annotated with comments, which helps seekers
- Design docs
	- Before writing code, Google engineers write their proposed design up in a doc, which is then reviewed
	- This should cover goals, implementation strategy and trade-offs between different alternatives considered
	- It acts as a historical record and can be useful for checking back
- Tutorials
	- The best time to write a tutorial is when you are new to the team
	- Number everything that you expect the user to do. This adds useful structure to the tutorial. Do not number system responses
- Reviewing documentation
	- > If you want to “test” whether your documentation works, you should generally have someone else review it
	- Check for *accuracy* (technical correctness), *clarity*, *consistency*. (Sometimes it is not possible to optimize for all of these, e.g. making something clearer might mean making it shorter and less complete / accurate)
- In documentation, redundancy is often useful, e.g. mentioning something in the middle of the document and in the final take-away section
- Deprecating documents: Make sure to add warnings to documents that are no longer updated and might be obsolete
- You could make use of technical writers when writing documentation for other audiences. When writing documentation for your own teams it usually is not necessary since your audience is familiar to you and has a lot of context. Another reason is that writing documentation yourself actually helps you understand things better. The value of technical writers comes out when you do not know the audience well
