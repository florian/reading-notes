### 23. Continuous Integration

- *Continuous integration* (*CI*) means developers frequently integrate their changes and each integration is verified by an automated build and tests
- This process is designed to catch problematic changes as early as possible
- *Version skew*: State of a distributed system where multiple incompatible versions of things exist next to each other
- CI is similar to monitoring production services: You want to find failures as quickly as possible and there can be flaky behavior
- CI at Google
	- The *Test  Automation Platform* (*TAP*) is responsible for running automated tests on changes
	- Every day it runs 4 billion test cases against 50,000 unique changes
	- Google has so many tests that it is no longer feasible to run all affected tests on every change. Instead, TAP batches together some changes. If a test fails, it then runs binary search to figure out which change is to blame
	- Problematic changes can automatically be rolled back if TAP is confident that it correctly identified the culprit
	- *Forge* is Google's system for distributed builds and tests. Running stuff on servers is usually faster than running it on the developer's machine
- Google Takeout is an interesting case study for CI. It integrated so many Google products that its CI was basically testing all of Google. Failure isolation is really difficult in such a situation
- You might think that you cannot invest resources into developing or maintaining a CI system. However, once you reach a certain scale it pays off many times and you do not really have a choice
