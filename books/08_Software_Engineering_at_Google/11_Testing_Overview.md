### 11. Testing Overview

- Testing supports the ability to change: When you add new features, redesign, refactor you can automatically test whether you did not break anything
- The later you fix bugs, the more expensive they are
- Writing tests scales much better than manual testing. At some point you have too many features to manually test all of them (think of Google Search)
- Steps in tests
	1. Set up expected environment
	2. Call into the system
	3. Verify the result
- Failing tests have to be fixed immediately for them to be useful
- Writing tests has a larger up-front cost than not writing them. In might even take you longer to write the tests than to do the actual change, but it will pay off since
	- you need to do less debugging in the future
	- there's increased confidence in changes
	- tests work as documentation (but it is executable and always up to date), as long as they are written in a clear way
	- the changes will be easier to review
	- it enables fast iteration in the future
- *Size* of a test: Time, memory, processes required to run the test
	- Classified into three groups
		- *Small*: Run in a single process (for some languages this is even restricted to single thread). They may not sleep, perform I/O operations or do anything else that's blocking. It is difficult to make these tests non-deterministic
		- *Medium*: Run on a single machine and may call localhost
		- *Large*: Run wherever the want. It might make sense not to run these as often as the other kinds of test as they might be flakier and take longer to run
	- Engineers are encouraged to write tests that are as small as possible, because that means they are easier to debug and faster to run
	- For some languages Google has tooling to automatically tag the size of a test
- Isolating problems in tests
	- Regardless of the size, tests should be hermetic and assume as little as possible about the environment
	- Tests should contain exactly the information needed to set them up, i.e. you should be able to learn everything important from reading the test
	- > We like to say that “a test should be obvious upon inspection.”
	- For the same reason, control flow statements (loops and conditionals) should not be used in tests
- *Scope* of a test: How many code paths are verified
	- Three groups, but not formally defined
		- *Unit tests*: Test focused on just a class or method
		- *Integration tests*: Testing interactions between a small number of components
		- *End-to-end tests* (or *functional tests* or *system tests*): Testing the validity of parts of the full system
	- The important aspect is not the code being executed but what is being validated
	- Google suggests 80% unit, 15% integration, 5% end-to-end tests
- Flaky tests
	- Become problematic at 1% flakiness
	- Google is at roughly 0.15%
	- You can re-run them to trade-off computing resources vs engineering time
	- But once they become too flaky, engineers lose confidence in the tests which is much worse than spending more engineering time on them
- > We are often asked, when coaching new hires, which behaviors or properties actually need to be tested? The straightforward answer is: test everything that you don’t want to break.
- It is important to also test for failure, i.e. what happens when something you depend on fails or when inputs are not exactly as you would expect them to be
- Code coverage is a useful metric but it is easy to game it: You could add a huge test which covers all lines but does not really tell you where something is failing. Google suggests only using small tests when computing code coverage
- "Wait-and-check" in tests is dangerous. Not only can it be flaky, but it really slows down tests. This is especially bad when it happens in testing helpers that are meant to be re-used
