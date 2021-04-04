### 17. Code Search

- > GSearch originally ran on Jeff Dean’s personal computer, which once caused company-wide distress when he went on vacation and it was shut down!
- Code Search is a separate web tool that is optimized for reading, e.g. clicks on a symbol can be used to find other instances of the symbol rather than editing it
- Developers often have multiple Code Search tabs open at the same time as writing code in their IDE. Code Search is the default code viewing platform at Google
- You can measure the impact of slower Code Search quite well. If each search were 1 second slower, that'd add up to a huge number of lost development time: 35 engineering days for the total queries across Google in one day. Thus, investing in Code Search makes a lot of sense
- Google developed its own compression scheme for the Code Search index
- Ranking
	- There are two kinds of signals
		- *query independent*: e.g. file length, number of views, number of references to the file (PageRank-like)
		- *query dependent*: e.g. literal string match
	- Ranking based on number of views can be self-reinforcing (i.e. there's an exploitation vs exploration problem)
	- *supplement retrieval*: Rewrite the query into more specialized queries  (e.g. for specific languages) and rank all of those. This is useful if you have to limit how many elements you can rank but want diverse results
- Code Search errors on the side of indexing too much (e.g. generated files) in orders to not lose developer trust
- Code Search finds deleted files. This is actually pretty important because otherwise developers do not really delete files but just move them into legacy directories in order to be able to still Code Search them
- Tokenization is problematic for code: "Searching" and "searched" are different and there's no agreed-upon tokenization for camelCase, snake_case or special code symbols
- This would make searching based on tokenization difficult. That's why Google's Code Search is based on regular expressions
- If you have substring search working (in practice: trigram search), you can extend it to regex search: The regex automaton is converted into a set of substring searches. (A few such regex queries will be really expensive but most will not be). There's a great [blog post](https://swtch.com/~rsc/regexp/regexp4.html) about how this works
- Less powerful versions of Code Search (or any tool) are/were fine as long as their limits were understood (*Note: This seems like a general takeaway*)
