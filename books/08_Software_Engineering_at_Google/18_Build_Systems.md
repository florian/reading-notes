### 18. Build Systems and Build Philosophy

- Google's Blaze build system was so popular that it was reimplemented several times by Googlers who left the company. In 2015, Google then open-sourced an implementation of Blaze called Bazel
- The need for a build system
	- Manually managing your build does not scale
	- When other developers join you, they will have slightly different development environments and you do not want to debug small differences all the time
	- Writing your own shell script to create your build will become more and more work, and will thus not scale. You would re-invent the wheel here if you keep working on your own build system
- *Task-based build system*: One defines tasks and declares dependencies between them (i.e. task A has to run before task B)
	- These systems give a lot of power to developers since tasks can essentially do anything
	- Hard to parallelize because it is unclear which tasks can be run at the same time (e.g. some might access the same database or try to acquire the same locks)
	- Incremental builds (i.e. not rebuilding everything after a small change) are difficult to do because it is unclear which tasks need to be re-run
	- It is easy to break the build: Let's say task A depends on task B which depends on task C which outputs certain files. If A happens to use those files and B decides that it does not need C anymore, the build would break for A
	- There can be race conditions, e.g. if A depends on B and C, which both write to the same output file. This does not just impact parallelity but also correctness / reproducibility 
	- Even though there are all these disadvantages, most build systems are task-based, e.g. grunt, rake, gradle, ant, maven
- > Though they often receive less scrutiny, build scripts are code just like the system being built, and are easy places for bugs to hide
- *Artifact-based build systems*: Instead of telling  the build system *how* to build, you just tell it *what* to build
	- Artifacts to build and their dependencies are described in a declarative manner
	- This is what Google's Blaze is based on
		- At the root of the source code hierarchy there's a *WORKSPACE* file with some meta information / imports from the outside
		- Each package has a *BUILD* file with the build targets
	- Because artifact-based systems know exactly what they are building, they can automatically figure out what to parallelize, how to do incremental builds and where there might be race conditions
	- As long as build targets are deterministic and only use their declared inputs / outputs, Blaze can automatically schedule them. That's even the case for custom targets
	- Blaze actually runs tasks in a sandbox to ensure that they only use the specified inputs and outputs
	- Because of the above, it actually does not matter on what machine you do Blaze builds. In fact, Google does Blaze builds in the cloud, distributed among a fleet of workers. This vastly speeds up builds
	- It can be expensive to port task-based builds to artifact-based ones. Instead, you should try to use artifact-based builds from the start
- Blaze / Bazel suggestions
	- Theoretically you could move your entire project into one big Blaze target. That'd make parallel and incremental builds impossible though. Instead, you should break up the project into sensible targets. Too many targets might also be suboptimal though as you would have to update dependencies a lot
	- Choose the minimum viable module visibility (*Note: Just like in coding*)
	- Blaze errors out when people are trying to use transitively imported modules (A->B->C and A tries to use C), at least when using Java and C++
	- Many build tools automatically fetch transitive outside dependencies. Google thinks that's a bad idea because you do not know which version of the transitive dependencies you will get. Instead, there's tools that generate files containing all transitive dependencies and exact version numbers. These can then be put into source control and be updated when necessary