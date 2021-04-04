## Part 3: Processes

### 8. Style Guides and Rules

- Goal: Encourage good behavior. Discourage bad behavior
- The definition of *good* and *bad* depends on what the organization values (e.g. consistency or use of new language features)
- Instead of asking what goes into the style guide, you should be asking why does something go into the style guide
- Guiding principles that rules need to follow:
	- *Pull their weight*: There is a cost to adding rules, as it makes the style guide longer and engineers will have to read more. Google thus does not include completely obvious rules, e.g. some can be directly handled by auto-formatters
	- *Optimize for the reader*: Code will be read more often than it is written (`@Override` is one example of optimizing for readers)
	- *Be consistent*: Just like how Google offices in the world work similarly (same Wi-Fi, video conferencing system, etc) code bases should be consistent. This makes moving between them easy. Also note that consistency makes shared tooling possible
	- *Avoid error-prone and surprising constructs*: They are just too easy to mis-use
	- *Concede to practicalities*:  Sometimes you have to do things differently for performance or for compatibility with outside systems. Your rules need to be flexible enough for that
- Three kinds of rules
	- Rules to avoid dangers
	- Rules to enforce best practices
	- Rules to ensure consistency
- Sometimes it is not super important what you choose but just *that* you choose, e.g. when it comes to indentation. That stops people from discussing and everyone can move on
- Guidance ("shoulds") can be useful to provide with the rules ("musts"). These include guides about specific topics as well as Google's "Tip of the Week"
- Applying rules
	- Automatic enforcement of rules is a good thing because it is more consistent than relying on people to enforce the rules
	- Of course not everything can (yet) be automatically checked though
	- Go included a standard formatter from the first release. This made it possible to enforce a common standard from the start
- Having autoformatters that are used by all code enables making automated changes to code at scale, since formatting is no longer a concern
