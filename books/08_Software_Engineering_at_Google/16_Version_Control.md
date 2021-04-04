## Part 4: Tools

### 16. Version Control and Branch Management

- Google uses *trunk-based development*, where all code lives in one repository and there are no dev branches. Google finds this to be particularly scalable
- *VCS*: *Version control system*
	- *Repository* (*repo*): Collection of files and metadata about them
	- > A filesystem is a mapping from filename to contents. A VCS extends that to provide a mapping from (filename, time) to contents, along with the metadata necessary to track last sync points and audit history
	- Version control allows software engineering to scale
	- Most VCSs have the same functionality at the core. However, how nicely they can be used can differ a lot and also depends on personal choice
	- *Centralized* VCS: There's a central server which keeps track of everyone's work. When you want to commit, you talk to the server. In the old days, you even talked to the server to lock files
	- (*Note: Kind of odd that the first VCSs were centralized and kept locks of files. Nowadays, systems are much more connected, so it feels like back then there were so many more incentives for decentralization*)
	- *Decentralized* VCS: Everyone maintains their own copy of the repo and can exchange their changes with anyone else
	- (*Note: Git is decentralized but this description makes me think that most people really use it in a centralized manner, with one central GitHub repository*)
	- Even decentralized systems often need a source of truth (i.e. which is the latest version to build on) in order for large projects to work out. Otherwise it will eventually become hard to merge in changes
- Branch management
	- Perforce keeps track of the change number that a change is based on and the one where it is merged into HEAD
	- *Dev branches* are in-between "done but not committed" and "this is what new work should be based on"
	- Google finds dev branches to work badly because merging becomes more work the longer you wait
	- Trunk-based development is an alternative where incomplete features are disabled by flags
	- *Release branches* can be used to keep track of the latest state that went into a release. When you want to add something to the release, you just sync it into the release branch
	- A better way to do things is continuous deployment though
- Version control at Google
	- Nearly all code is stored in a monorepo
	- (*Note: I summarized the paper about that [here](/papers/003_Why_Google_Stores_Billions_of_Lines_of_Code_in_a_Single_Repository.md)*)
	- Google implemented its own version control system inspired by Perforce. This allows it to scale to Google-scale and also allows Google to really customize its behavior (e.g. OWNER files)
	- Only one version per third party library is allowed into the monorepo. It is then updated for everyone at the same time, which makes dependency management very easy
	- > Continuing the parallel between filesystem format and VCS, it’s easy to imagine deciding between using 10 drives to provide one very large logical filesystem or 10 smaller filesystems accessed separately. In a filesystem world, there are pros and cons to both
- Even if you use many repos, you probably want to pin third party dependencies to a single agreed-upon version
