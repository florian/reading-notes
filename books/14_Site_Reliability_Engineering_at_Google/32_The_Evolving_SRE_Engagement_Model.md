## 32. The Evolving SRE Engagement Model

- The other chapters in this book mostly deal with what happens once SREs get involved. This chapter discusses *how* SREs get involved
- Generally, the earlier teams consult with SREs the faster and cheaper the process is. Think of it like fixing a bug: the earlier you find and debug it, the easier it is
- *fait accompli* environments (where everything is already built) are hard to change
- Instead of consulting teams individually, it can be more efficient to build a platform that other teams can then use in a self-serve manner
- *Production Readiness Review* (*PRR*)
  - is the review performed at the start of SRE engagement
  - discusses system architecture, dependencies, metrics, monitoring, emergency responses, capacity planning, change management, performance goals
- Services that do not require high reliability do not need SRE support
- There are three models of SRE engagement
  1. Simple PRR Model: On-board projects to SREs that are already built
  2. Early Engagement Model: Engage with projects from a very early stage on, since it is more efficient if you can design projects with reliability in mind from the start on
  3. Frameworks and SRE Platform: Instead of engaging in detail with each project, build a platform that other teams can build on. This allows for better scaling of resources and much lower start-up costs. It also means teams do not have to decide between "no SRE support at all" and "full SRE support", since this platform approach sits somewhere in the middle
- The second and the third of these build on the lessons learned from the prior engagement model
- If a service no longer needs SRE (e.g. because it never really took off or because it just builds on frameworks kept stable by SREs), then it is possible that SREs disengage from the service
