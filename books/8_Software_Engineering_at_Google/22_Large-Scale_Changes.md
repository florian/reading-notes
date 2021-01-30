### 22. Large-Scale Changes

- A *Large-Scale Change* (*LCS*) is a set of changes that is logically related but cannot be submitted at once in practice because of its size, i.e. too many reviewers would be necessary or too many tests would have to be run
- If no monorepo is being used, committing a LCS in one change may not even be technically possible
- Google hat tooling for automatically generating LCS
- Kinds of LCS
	- Cleaning up antipatterns
	- Replacing deprecated things
	- Small improvements
	- Porting users to a new system
- LCS allow domain experts to work on the changes, instead of requiring each team to get familiar with the details. This allows experts to apply their knowledge in a scaled up way. It is also important because for many teams the benefits might not be large enough to justify spending the time of learning the details
- Committing LCS in an atomic way is infeasible:
	- There's technical limitations, e.g. with multiple repos
	- Merge conflicts increase as you change more parts of the codebase at the same time
- *Haunted graveyards* are old systems that are "frozen in time" because nobody dares to make a change since they could break the system so easily. Haunted graveyards should be avoided at all costs. Tests are especially helpful for this. Haunted graveyards are especially bad for LCS
- LCS is made possible by several things already mentioned in the book earlier
	- Engineers can see all parts of the monorepo
	- Everything in the monorepo should be tested well
	- All code in the monorepo is autoformatted, meaning LCS can be formatted in the same way
- For LCS to make sense, the human effort involved has to scale sublinearly with the size of the monorepo
- > As a rule of thumb, we’ve long held that if a change requires more than 500 edits, it’s usually more efficient for an engineer to learn and execute our change-generation tools rather than manually execute that edit. For experienced “code janitors,” that number is often much smaller. 
- Rosie is Google's platform for performing LCS. It splits LCS into smaller, reviewable changes
- LCS process at Google
	1. Authorization: Author describes their desired LCS and has an owner of the API approve it
	2. Change creation: Author implements their change, as automatic as possible
	3. Sharding: Rosie splits up the LCS into many small changes based on project boundaries, finds fitting reviewers and sends out the changes for review
	4. Cleanup: After all changes have been submitted, we need to make sure that the thing we just removed/refactored will not get re-introduced to the codebase later on. This generally means that checks need to be added to the static analyzer system
