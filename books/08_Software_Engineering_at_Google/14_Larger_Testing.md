### 14. Larger Testing

(*Note: This felt like the most abstract chapter of the book. Maybe that's because I don't have direct experience with many of the tests here. Maybe it's also because the chapter bundles a lot of losely related things together.*)

- *SUT*: *System under test*
- *Fidelity*: How reflective the test is of real behavior
- Common gaps of unit tests
	- Test doubles that are not reflective of the actual implementation
	- Code depends on a config, which is different in production / when deployed
	- > At Google, configuration changes are the number one reason for our major outages
	- Issues that only arise under load (i.e. load or performance testing)
	- Issues that do not exist in a "vacuum" but only when multiple components are connected (i.e. you want to test integration)
- Large tests are often slower, less reliable (e.g. flaky) and might not be as scalable (i.e. you can't run a ton of them for presubmits)
- Structure of a large test
	- Steps
		1. Obtain system under test
		2. Prepare necessary data
		3. Use data to perform action in the system
		4. Verify behavior
	- One can record real traffic and replay it to automatically test services
	- Large tests require two kinds of data
		1. *Seeded data*: Used to pre-initialize the system
		2. *Test traffic*: Data given to the system during the execution of the test
	- Hand-crafting test data is harder for large tests since data might be required for multiple services
- Types of larger tests
	- Functional testing of multiple binaries
	- Browser and Device Testing: E.g. accessing stuff via the UI, just like how users would
	- Performance, Load and Stress Testing: Requires an environment very close to the real one, as some issues only surface with heavy traffic
	- Deployment Configuration Testing: Often smoke tests
	- Exploratory Testing: Manual testing looking for questionable behavior. This is a bit like manual fuzzy testing but does not scale, so any found issues should be covered with automatic tests in the future
	- A/B Diff Regression Testing: You send real traffic to the new system, record any changes and (manually) audit whether they are expected. You can also A-A test to find flaky / non-deterministic behavior, or A-B-C test to compare production, HEAD (accumulated changes) and the new change
	- User Acceptance Test: Users / customers write specifications that can automatically be parsed and tested
	- Probers and Canary Analysis: Smoke tests of the production system. This should only perform read-only actions to not mutate the system
	- Disaster Recovery and Chaos Engineering: Infrastructure is turned off on purpose to see how robust the connected systems are. The idea behind this is that teams should not expect reliability. Chaos Engineering is a "continuous" form of this where systems might have small failures all the time
	- User Evaluation: Dogfooding, A/B live experiments, paid human raters. These are often important when there's no *correct* behavior, only better or worse (e.g. machine learning) 
- A common mistake is to write codes that test "working as *implemented*" rather than "working as *intended*"
- Developer workflow
	- Google puts large tests into postsubmit continuous integration, since these tests are often too flaky or resource-intensive
	- Diff tests can mean that someone needs to approve the diffs inside a normal code review
