# Software Engineering at Google [[pdf]](https://arxiv.org/abs/1702.01715)

*Henderson, Fergus. "Software engineering at Google." arXiv preprint arXiv:1702.01715 (2017).*

This paper gives a high-level overview of software engineering principles at Google.
This includes technical topics as well as project and people management concepts.
Since most of this is only discussed on a very high level, the paper is easy to read and gives an interesting glimpse into how things work at Google.

## Software development

### The Source Repository

- Most of Google's code is stored in a [single repository](papers/3_Why_Google_Stores_Billions_of_Lines_of_Code_in_a_Single_Repository.md)
- The notable exceptions are Chrome, Android and some security-critical pieces of code
- Write access is more controlled than read access: Directories list owners and one of them needs to approve any changes to the respective directories. The owner status also transfers to any subdirectories
- Most development happens at the head of the repository, not on branches. This minimizes the amount of merging that needs to be done
- Automated systems run tests when new code is added
- Code that does not pass tests is generally quickly rolled back
- Each subtree needs to have at least two owners

### The Build System

- By using the standardized build system *Blaze*, any Google engineer can easily build code in any part of the repository
- Declarative *BUILD* files describe the rules that Blaze uses to build the system
- BUILD files for Go programs can be generated automatically
- Each build is typically distributed across hundreds or thousands of machines. This makes it possible to build extremely large programs quickly
- Build steps need to be pure. They should not have side effects
- Builds are deterministic. They always produce the same results. The build team made sure to remove any kind of timestamps from builds
- Build results are cached in the cloud

### Code Review (CRs)

- Google has great web-based review tools
- All changes must be reviewed by at least one person
- Code review discussions are automatically copied to a mailing list
- The repository has an *experimental* section for quick prototyping. Code reviews are not enforced here
- Developers are encouraged to keep individual changes in individual code reviews small

### Testing

- All code in production must have unit tests
- The review tool highlights files without corresponding tests
- Integration, regression and load testing are also widely done

### Bug Tracking

- CRs can be associated with a bug number
- Some teams have individuals that are responsible for prioritizing and assigning bugs. In other teams, this is done during team meetings

### Programming languages

- There are five official languages at Google: C++, Java, Python, Go and JavaScript
- All of these languages have style guides at Google
- Developers that demonstrated that they can write great code in a language, get *readability* rights. During code reviews, at least one reviewer with readability status in the respective language needs to approve the change. To get readability status, one's code is thoroughly reviewed for some time
- For communicating between different programs and languages, [Protocol Buffers](https://developers.google.com/protocol-buffers/) are used
- Protocol Buffers are also integrated into Google's RPC libraries
- Many tools at Google are language-independent. This makes it very easy to switch to other projects or languages

### Debugging and Profiling tools

- Google servers have libraries for debugging built-in
- For example, if something crashes, one can change command-line flags in a web interface and run everything again
- This greatly decreases how much conventional tools like gdb are used

### Release engineering

- Some teams have dedicated release engineers, but mostly this is done by regular software engineers
- Releases are often done weekly or fortnightly, sometimes even daily
- Regular releases keep engineers motivated and make it easier to change ideas based on user feedback
- To deploy a release, dedicated release branches are used
- First, the change is typically pushed to a *staging* server. The developers of the team test the change here. Sometimes a part of the real traffic is used to check if the staging server reacts in a good way. However, the results are not send back to actual users
- Next, the change is sent to *canary* servers. A subset of the user traffic is processed by these servers and users do see the responses
- If everything works fine until here, a gradual roll-out to production is performed. This could take a few days

### Launch approval

- Changes visible to users need approval from people from the respective core engineering team
- There might be more approvals necessary, e.g. because of legal, privacy or security requirements
- Google has an internal launch tool for tracking what reviews are still required

### Post-mortems

- When there is a major problem in production systems, the people involved write a *post-mortem* document
- This document describes what happened and how it could be avoided in the future
- The focus is on the problem and how to fix it in the future, not on blaming people for it

### Frequent rewrites

- A lot of software at Google is rewritten from time to time
- This might seem like a waste of resources but it has some advantages
- Knowledge about the system that is rewritten is transferred to new people
- It ensures that code is written using modern technologies

## Project management

### 20% time

- Engineers can spend 20% of their time working on a side project
- Makes it easy to experiment with different ideas
- This also keeps engineers happy, which accounts for much more than 20% of productivity

### Objectives and Key Results (OKRs)

- Individuals and teams explicitly document their goals and ways to measure their progress
- This is done at all levels of the company
- Each objective is given a score between 0 (no progress) and 1 (completely done) at the end of each quarter
- The desired average score is 0.65: Goals should be ambitious enough that they cannot all be completed
- OKRs have no impact on performance evaluations or compensation

### Project approval

- Works differently in various parts of the company
- Sometimes top-down (management makes a plan), sometimes bottom-up (engineers decide)

## People management

### Roles

- There are separate ladders for engineering and management
- Research teams are embedded into product teams
- The main roles are:
  - *Engineering Manager*: Generally former software engineers that manage people. One does not have to be a manager to lead other people though
  - *Software Engineer (SWE)*: Most of the people in software engineering are in this role. At higher levels, showing leadership does not have to mean managing / leading people
  - *Research Scientist*: Very strict hiring criteria. Most PhDs are not hired as research scientists but as SWEs. Research scientists are evaluated on their research contributions, e.g. publications. They generally work along SWEs
  - *Site Reliability Engineer (SRE)*: A mixture between software engineering and system administration. The software engineering requirements are a bit lower and can be compensated by Unix or networking knowledge
  - *Product Manager*: They coordinate software engineers and work out new feature ideas. They do not write code themselves
  - *Program Manager / Technical Program Manager*: Instead of managing a product, they manage projects or processes. This role is rather rare

### Training

- New googlers go through introductionary classes called *codelabs*
- Google supports studying at external institutions as well as taking online courses

### Transfers

- Transferring to a different team inside the company is encouraged
- Sometimes, SWEs also do 6-month rotations as SREs

### Performance appraisal and rewards

- Engineers can give each other peer bonuses, a small cash bonus for doing something above their actual work
- For promotion, people have to nominate themselves or have to be nominated by their manager
- The performance of managers is partly assesed by asking people in their team for feedback

---

## Related links

- [Hacker News discussion](https://news.ycombinator.com/item?id=13619378)
