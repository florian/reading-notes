## Appendix B – A Collection of Best Practices for Production Services

- Fail sanely: Continue to serve results if things fail, e.g. by serving based on a previous configuration file
- Progressive rollouts: Unless a rollout is an emergency, it must proceed within stages to notice problems early
- Define SLOs
- Define error budgets and hold yourself accountable to them
- Have a well-defined monitoring system
- Write postmortems
- Plan ahead for capacity
- Plan for what happens when services are overloaded or fails
- SREs should spend at most 50% of their time on toil and should otherwise work on automating things
- (*Note: This appendix was actually a pretty good summary of the book*)
