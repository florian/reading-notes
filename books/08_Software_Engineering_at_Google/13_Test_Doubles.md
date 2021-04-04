### 13. Test Doubles

- A *test double* stands in for a real implementation to make it easier or faster to test certain things
- Test doubles can make it possible to write *smaller* tests, since e.g. you do not need a real database or server with their own processes
- For doubles to be easy to use, code needs to be designed with testing in mind. E.g. when you are using a database, it needs to be possible to replace it with a double
- Motivating example for doubles: Imagine testing a credit card payment server with fees for transactions
- Overusing mocking frameworks can lead to worse tests since they are harder to adapt and might not catch all bugs
- Dependency injection
	- allows you to replace real implementations with fake ones, called *seams*
	- If you are using the real implementation, dependency injection can also useful to avoid having to instantiate all dependencies yourself
- Types of doubles
	- *Fake*: lightweight implementation of the API but would not be suitable for production, e.g. in an in-memory database
	- *Stub*: Double that you tell exactly what to return for which inputs. This is typically done through mocking frameworks
	- Interaction testing: There's no implementation, you just check that the object is called correctly, e.g. the right number of times or with the expected arguments. This is sometimes called a *mock*, which is confusing because using doubles in general is often called *mocking*. It should also be avoided because it leads to brittle tests (since it might be testing implementation details)
- Be careful with using doubles
	- Your first choice should be to use the real implementation as this is clearer and will give you more certainty that the system works as expected for users. Only use doubles if there are good reasons
	- Google developed a `@DoNotMock` annotation that lets API owners declare that a better alternative exists
	- This is because needlessly using mocks can make it harder for the API owner to update interfaces, since mocks might have behavior that the real implementation would never have (e.g. certain return values)
	- > A real implementation is preferred if it is fast, deterministic, and has simple dependencies
	- Non-hermetic code is problematic in tests, e.g. a web server depending on some external service or the system clock. These should always be replaced with doubles
- Fakes
	- Require maintenance
	- The *relevant* behavior should be identical between fake and real implementation. E.g. for hashing it might not be relevant what the exact hashes are, but that they are deterministically assigned
	- Should be tested. It might be nice to run fake and real implementation against the same tests to make sure they do not diverge (*contract tests*)
- Stubbing
	- > A key sign that stubbing isn’t appropriate for a test is if you find yourself mentally stepping through the system under test in order to understand why certain functions in the test are stubbed
	- Lead to brittle changes because you often depend on implementation details (i.e. is it really part of the API contract that the object is called this way?)
	- Has no internal state, which is sometimes required for tests (think of a database)
	- A sign that you overuse stubs is when your stubs have no direct apparent relationship to the assertions. In this case, a fake might be a better choice
- Interaction testing
	- Prefer testing state instead of interactions as this depends less on implementation details
	- Examples:
		- Instead of checking that a database save operation was called correctly, check what the database contains
		- Instead of checking that a sorting algorithm was called with the correct input, check that the result was sorted
	- > Some people at Google jokingly refer to tests that overuse interaction testing as *change-detector tests* because they fail in response to any change to the production code, even if the behavior of the system under test remains unchanged
	- Reasons to use interaction testing
		1. Other ways to test are infeasible
		2. How the method is being called is actually part of the contract, e.g. you want a callback to be called in a specific way or you do not want multiple calls because of caching
	- > If you are not able to perform state testing in a unit test, strongly consider supplementing your test suite with larger-scoped tests that do perform state testing
	- You only need interaction testing for functions with side effects. If the function has no side effects, you will be able to test by asserting something about the return value
