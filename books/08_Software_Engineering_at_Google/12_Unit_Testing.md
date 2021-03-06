### 12. Unit Testing

- *Unit test*: Narrow scope, tests a single method or class
	- Easy to write at the same time as implementing the code
	- Make it easy to pin-point bugs since they are simple and isolate components
	- Serve as documentation
- Properties of bad tests
	- *Brittle* tests break when unrelated changes are made
	- *Unclear* tests make it difficult to pin-point the problem
- If tests have to be changed whenever changes are made, they are not really "automatic" tests
- The ideal test is *unchanging*: It only needs to be changed when the actual requirements of the system change
- Test via public APIs (i.e.: how to call into the system)
	- Tests should use public APIs, just like how any users would
	- Implementation details should not be tested
	- These tests are less brittle because them breaking means that something would also be broken for users
- Test state, not interactions (i.e.: how to assert)
	- Two options
		- *State testing*: Verify that the system looks the way you expect it to
		- *Interaction testing*: Verify that the system took the expected sequence of actions in response to you calling it
	- Many tests do both
	- Interaction testing is more brittle since you do not necessarily care *how* the system arrived in its desired state but only *what* that state is
	- This is quite similar to why you do not test implementation details
	- Relying too much on mocks is often the reason for why interactions are tested too much
- Writing clear tests
	- > A test is *complete* when its body contains all of the information a reader needs in order to understand how it arrives at its result. A test is *concise* when it contains no other distracting or irrelevant information
	- Repeating yourself in tests is fine if it makes them complete
	- One test per *behavior* tested, not per *method* tested
	- Behaviors can be expressed using given-when-then: *Given* state X, *when* Y happens, *then* we expect to see Z
	- > Given that a bank account is empty, when attempting to withdraw money from it, then the transaction is rejected
	- > The mapping between methods and behaviors is many-to-many
	- You should not have to write tests to make sure your tests are correct. That's why control flow should not be used in tests
- Code sharing in tests
	- Test should be *DAMP*: *Descriptive and Meaningful Phrases*
	- Duplication is okay as long as it makes the test easier to read
	- Relevant values should live inside the test. Everything else that needs to be set can be moved to helper functions (e.g. `newAccount`)
	- One can randomize the default values to make sure nobody really depends on them (*Note: It seems like it'd be better to really check that they are not accessed, rather than making the test flaky*)
	- The same idea of repeating yourself for clarity applies to test setup
	- If you need helpers for verifying behavior, they should assert only one conceptual fact
