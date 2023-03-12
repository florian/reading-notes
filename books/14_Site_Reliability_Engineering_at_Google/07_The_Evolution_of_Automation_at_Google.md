## 7. The Evolution of Automation at Google

- Value of automation
    - Consistency
    - Easy to update and extend
    - Saves time
- Hierarchy of automation as it commonly happens
    1. No automation: A developer manually takes care of things
    2. Custom script: The developer has a script in their home directory that deals with the problem
    3. Generalized script: The script is general enough and submitted to version control
    4. Integrated script: The script ships with the software failing
    5. Integrated fix mechanism: The software has integrated behavior for automatically fixing the issue
- The mandatory xkcd on automation: [https://xkcd.com/1205/](https://xkcd.com/1205/)
(*Note: Without this, the chapter would have felt incomplete to me*)
- On being able to use SSH to fix issues:
> A growing awareness of advanced, persistent security threats drove us to reduce the privileges SREs enjoyed to the absolute minimum they needed to do their jobs
- Systems like Borg were immensely useful for automating infrastructure
- Automation can also enable failure at scale. If something goes wrong and it is automated, it might be much more damaging than something an individual could easily do. It is important to build in the right safeguards to prevent this
