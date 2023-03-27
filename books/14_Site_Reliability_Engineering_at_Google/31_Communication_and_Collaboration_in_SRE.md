## 31. Communication and Collaboration in SRE

- Today, SRE spends more time supporting its individual systems than it spends on cross-production work. That's on purpose since it is more scalable
- *Production meeting*: Meeting to discuss the state of a service
  - In the meeting, anything related to how well things currently run in production is discussed
  - In meetings where most of the people in the call are in one specific meeting room, it can be a good idea to have the moderator be someone from a different site, in order to avoid the discussion only happening in the site where the team has the largest presence
- > In Google, TLs can do almost all of a manager's job, because our managers are highly technical, but the manager has two special responsibilities that a TL doesn't have: the performance management function, and being a general catchall for everything that isn't handled by someone else
- *Viceron*: a monitoring service and dashboard
- SREs teams are very conservative about adopting new monitoring services since new things are generally less stable initially
- > For situations where neutrality is important, it's advantageous to hold team summits at a neutral location so that no individual site has the "home advantage"
- It can be a good idea to time-bound decisions if one could discuss forever and the result would just be different by a small amount
- The collaboration between SRE and product teams should ideally start before any code is written. That way, SRE can make sure a scalable architecture is used
