# Why Google Stores Billions of Lines of Code in a Single Repository [[pdf]](https://ai.google/research/pubs/pub45424) [[video]](https://www.youtube.com/watch?v=W71BTkUbdqE)

## Google-scale

- The vast majority of Google's code is stored in a single repository
- As of 2015
    - 1 billion files
    - 35 million commits
    - 86TB of data
    - 2 billion lines of code
    - 800k queries per second
    - Code similar to the size of the Linux kernel is written each week
- Number of automated commits is increasing
- More than just code is stored
    - Generated source code
    - Config files
    - Documentation
    - Supporting data files
- Google did not find a commercial or open-source version control system that could handle the scale
- To still be able to use a monorepo, Google developed *Piper*

## Background

### Piper and CitC

#### Piper overview

- Piper was first implemented on top of [Bigtable](https://static.googleusercontent.com/media/research.google.com/en//archive/bigtable-osdi06.pdf) and then on top of [Spanner](https://www.usenix.org/system/files/conference/osdi12/osdi12-final-16.pdf)
- Distributed over 10 data centers
- The [Paxos](https://en.wikipedia.org/wiki/Paxos_(computer_science)) algorithm is used to guarantee consistency
- A lot of redundancy to minimize latency
- Before Piper, there was a single Perforce instance handling everything

#### Piper workflow

- Developers create a local copy of files before changing them
- These files are stored in a *workspace* of the developer. This is comparable to a local clone in Git
- Updates from the Piper repository can be pulled into the workspace
- A snapshot of the workspace can be shared for review
- Files are only committed to the central repository after a code review

#### Clients in the Cloud (CitC)

- Workspaces are directories in the file system
- The full Piper repository is shown, changes are overlaid
- There is no need to clone or sync local state
- This means that workspaces are small but it is still possible to see the entire codebase
- All writes to files are stored as snapshots. Snapshots can be named and restored
- The workspaces are available on any machine that can connect to the cloud, which makes it easy to switch machines
- It can be configured to automatically run tests and submit the changes once the code reviewer approved everything
- The Google CodeSearch tool supports editing files through CitC workspaces
- Piper could be used without CitC, but few people do so

#### Workflow in a nutshell

1. Sync user workspace
2. Write code
3. Code review
4. Commit

### Trunk-based development

- Most developers work at the *head*, the most recent version of all the code
- Other names for this are the *trunk* or the *mainline*
- After committing, the code is immediately usable by everyone else
- One benefit is that merges are less common
- Branches are usually used for releases
- Releases are generally a snapshot of head plus additional cherry-picks
- Development branches are avoided by using conditional flags and config files
- A bit more complexity for developers but less merge conflicts
- Eventually old code is deleted and flags can be removed

### Google workflow

- All affected dependencies are automatically rebuild when committing
- If the change breaks something, a system is in place to automatically undo it
- Changes are automatically tested and analyses before they are added to the codebase
- Every directory has owners that need to approve changes affecting those directories
- Google has a large static analysis infrastructure. Suggested changes can often be applied using a single click
- *Rosie* is a tool that helps with refactoring existing code. Changes are split into individual patches so that they only affect individual directories

## Analysis

### Advantages

#### Overview

- Unified versioning
- It is straightforward and encouraged to share code
- Dependency management is simple
- Atomic changes can be applied to many parts of the codebase at the same time
- Refactoring across the codebase is simpler
- Collaboration across teams is easier
- Team boundaries are more flexible

#### Dependencies

- One can directly depend on code from other teams. It is already in the same repository
- It is not necessary to version dependencies
- There is no [*diamond dependency problem*](https://www.well-typed.com/blog/2008/04/the-dreaded-diamond-dependency-problem/)
- The person updating the library can update all affected dependencies at the same time

#### Testing out changes

- Maintainers of core libraries or compilers can test very easily how others are affected by their changes
- Since everyone works in the same repository, they just look at all projects there
- Codebase-wide cleanups become possible
- Automated code analysis tools can be used more easily

#### Browsing the codebase

- Each team has a directory structure that serves as the project's namespace
- This also makes collaboration easier, as it is simple to see what others are working on

### Costs and trade-offs

- Three categories of costs
    - Large investment in tooling is required
    - The codebase becomes complex
    - Additional effort needs to be invested in code health
- Custom tools for IDEs and editors were developed
- Standard tools like grep do not work anymore on this scale
- Google invested a lot in making [code search](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/43835.pdf) easier
- Dependencies that are not really necessary can make things a lot more complicated, as library owners need to update more code
- Developers write less documentation because they can just refer to the code
- Some developers make their code depend on implementation details because they can see everything, not just the API
- There needs to be a distinct folder for open source to avoid duplicating dependencies

## Alternatives

- Git
    - Used by Chrome and Android teams
    - Cannot be used with a monorepo because ` git clone` always downloads the entire repository
    - Git developers use many small repositories
    - Android is split up in 800 Git repositories
    - This would not work for the existing monorepo
- Mercurial
    - Google is experimenting with Mercurial
    - Collaboration with the Mercurial community and people from other companies

## Conclusion

- Google decided on the monorepo approach in 1999
- There were several occasions where Google thought about switching to a different system but always decided against it
- This approach is only well-suited for companies where not much of the code is hidden
- A lot of tooling was required to make this approach scale with Google's growth
