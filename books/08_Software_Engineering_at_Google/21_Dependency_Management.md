### 21. Dependency Management

- *Dependency management* refers to managing external dependencies and how to describe, update and include them. Managing internal code on the other hand is just source control
- *Diamond dependency problem*: A depends on B and C which each depend on D. If D is now updated, and B / C use different versions of D, then it is unclear which one to include
- C++ cannot deal with diamond dependencies at all (*One Definition Rule*)
- Some languages can handle this to some degree, e.g. Java has mechanisms to rename symbols of D (*shading*). However, this only works for renaming functions and fails as soon as the same types are passed from A->B->D and A->C->D
- Potential fixes
	- Find a versions of the dependencies that match all requirements
	- Locally patch the dependencies
- Boost is a proving ground for new C++ features and does not even attempt to promise stable interfaces across releases
- 4 ways to approach the dependency management problem
	1. Nothing changes (*The Static Dependency Model*)
		- Dependencies cannot break if they are never updated
		- Ignores the fact that sometimes you need to update for security fixes, bugfixes, or because of platform changes
	2. *Semantic Versioning* (*SemVer*)
		- Three decimal-separated integers denoting *major*, *minor*, *patch*
		- *Major*: Breaking changes
		- *Minor*: New functionality which does not break existing usage
		- *Patch*: No API changes at all and just low-risk fixes
		- You can describe version dependencies in terms of these, e.g. major version is fixed but any minor/patch updates are fine
		- Given a dependency network, a SemVer solver can try to find a set of versions that meets all requirements (which works using SAT solvers). However, there exist dependency networks without any solutions
	3. Bundle what you need
		- An organization finds a set of compatible dependencies and publishes a bundled version of those
		- This is essentially what Linux distributions are
		- > Distributors are the engineers responsible for proposing a set of versions to bundle together, testing those to find bugs in that dependency tree, and resolving any issues
		- For outside users, this simplifies depending on a large network of dependencies to depending on one distribution
	4. Live at Head
		- > the dependency-management extension of trunk-based development
		- When you update a dependency, you update everything related to it in the repository
		- This requires having good test coverage and CI, so you can figure out if upgrading breaks something
- Finding a compatible set of versions is [NP-complete](https://research.swtch.com/version-sat)
- SemVer limitations
	- Semantic versions are really *estimates*. Even if developers set major, minor, patch to the best of their knowledge, they might not be correct
	- Hyrum's Law comes into effect here: If someone depends on a function being sufficiently fast or on a specific logging output, even a patch might be a breaking change for them
	- Especially as scale comes into play, these things will eventually happen
	- SemVer could overconstrain: Maybe you do not use any of the things that had breaking changes, so no major bump would be necessary for you
	- Incentives for SemVer developers might not be great. E.g. if a developer is also a user at the same time, then they have an incentive to make breaking changes that help them
	- Russ Cox suggested *[Minimum Version Selection](https://research.swtch.com/vgo-mvs)* (*MVS*): Instead of choosing the latest versions that satisfy all constraints, choose the ones that are closest to what you originally used while developing. This means that accidental breaking changes are less likely
	- SemVer might work well for a few dependencies but it seems unlikely that it works at Google scale
	- There's also a more fundamental problem here: We are solving an NP-complete problem based on estimates. It seems like conceptually this is already a broken solution
  - > SemVer is a lossy-compression shorthand estimate for “How risky does a human think this change is?"
- Dependency management with infinite resources
	- Open source users of packages are known. So are in-house ones
	- Assuming infinite resources, when we want to make a potentially breaking change, we could find everything that uses the package and run their tests
	- This  could be a data-driven way of figuring out if a change is actually breaking
- > Providing a dependency isn’t free: “throw it over the wall and forget” can cost you reputation and become a challenge for compatibility. Supporting it with stability can limit your choices and pessimize internal usage
