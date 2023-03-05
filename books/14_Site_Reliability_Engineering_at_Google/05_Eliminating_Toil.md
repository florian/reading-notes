## 5. Eliminating Toil

- > If a human operator needs to touch your system during normal operations, you have a bug
- Toil
    - *Toil* is operational work that needs to be done
    - Having too much toil prevents SREs from long-term engineering work
    - Toil is generally manual, repetitive, automatable, reactive, has no enduring value, and its costs grow linearly with the service
- At most 50% of an SRE's time should be spent on toil. Otherwise it is not feasible to automate enough to have the number of SREs grow sublinearly with the service
- You can compute time spent on toil by registering people as on-call. Generally, only on-call people should handle toil
- Spending too much of your time on toil also means career stagnation at Google. Google rewards grungy work only when it is absolutely required and cannot be automated well
