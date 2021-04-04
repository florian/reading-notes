### 20. Static Analysis

- *Static analysis* tools analyze source code without executing it. They can find bugs, antipatterns, typos and similar problems
- At Google, static analysis only runs on new code (i.e. code that is being reviewed). This is to avoid forcing developers to refactor their old code all the time
- Historically, people tried to decrease false negatives of static analysis tools. Google however focuses on reducing false positives to keep the tools usable at scale. Before Google started doing this, many static analyzers were unusable in practice
- Security-related analyses are an exception where reducing false negatives is more important
- Google actively measures how useful different static analyzers are
- Static analysis tools can directly suggest fixes, making it easier for users to act on the suggestions
- The false positive rate of static analyzers at Google is 5%. Each analyzer needs to be below 10% to be enabled by default
- `IfThis‐ThenThat` markers can be put into code to tell a static analyzer that certain files have to be changed in tandem
- Static analyzers have clear feedback channels that can be used when they mistrigger
- Users customization is often a way of hiding bugs that should actually be fixed. For example, users might just disable analyzers that overtrigger too much when in fact the tool should be fixed instead
- Changelist descriptions containing `DO NOT SUBMIT` will be blocked by a static analyzer
- When possible, static analysis should be pushed into the compiler to let users know of problems early. This is only possible for analyzers with virtually no false positives though. Furthermore, all potential issues already need to be cleaned up in the existing codebase
- Static analyzers allow code reviewers to focus on issues that are not mechanically verifiable
- Having domain knowledge experts write static analyzers means that improving code quality can scale much better: One expert is enough to ensure that the rest of the company follows certain best practices
- One can also display static analysis results inside IDEs. This requires analyzers that are much less aggressive though. For example, the warning that a function is unused might not be helpful since maybe the user has just not gotten to implementing the function calls yet
