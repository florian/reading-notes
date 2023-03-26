## 30.  Embedding an SRE to Recover from Operational Overload

- Some SRE teams get so busy with reactive work that they have no time to automate things
- To fix the situation, it can make sense to embed an additional SRE into the team. This SRE is supposed to do nothing else but to automate and to figure out what went wrong
- > the goal of the SRE model is to *only* introduce more humans as more complexity is added to the system
- Step 1 for the newly added SRE: Learn the service and get context
    - Find out why the team was not able to automate more at an earlier time
    - Some teams do not sufficiently invest in automation since they believe their service is tiny and scaling will not matter
    - There can be many more reasons ranging from the service unexpectedly having become more important to not having followed up on earlier postmortems
- Step 2: Share context
    - Classify toil and prioritze with the team
    - Write a postmortem on what went wrong
- Step 3: Drive change
    - Start with the basics: Everything should have an SLO
    - Ensure that previous postmortems are clearly followed up on
    - Explain your reasoning behind why certain things should be automated
    - > After you leave, the team should be able to predict what your comment on a design or changelist would be
    - Ask leading questions (which is a different thing than *loaded* questions). These should get people to think back to basic principles
- *postvitam*: Explaining the critical decisions that led to success
