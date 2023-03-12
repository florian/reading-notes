## 8. Release Engineering

- Philosophy when it comes to releasing
    - Self-service: In order to scale, teams must handle their own releases
    - High velocity: Changes should be rolled out as quickly as possible. How quick that is depends on the projects. For some it might be on every commit that passes presubmits
    - Hermetic builds: Builds should be independent of the environment they are executed in
    - Release policies: There should be access control to determine who can release what
- *Rapid* is Google's release system
    - When a release happens, a branch is created that tracks what goes into the release. One can cherry-pick into it
    - Software is shipped to production machines using the *Midas Package Manager* (*MPM*). MPM builds using Blaze, maintains a hash of the build, and signs it
    - Rapid is configured using Blueprint config files
