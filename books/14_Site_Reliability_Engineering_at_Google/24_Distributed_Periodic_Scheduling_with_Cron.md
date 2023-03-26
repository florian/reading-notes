## 24. Distributed Periodic Scheduling with Cron

- *cron* is a Unix util for scheduling jobs to run periodically
- There’s a lot of flexibility in terms of how those periods can be expressed
- Failures are hard to reason about since cron could run anything. Some programs are super important to run, others are not crucial to run every time. Some programs can be executed one too many times without side effects, others have significant one
- Letting one specific server be in charge of running cron jobs makes it too easy to make everything fail
- Instead it is preferable to move scheduling to the data center scheduler which then distributes jobs to workers
- Google implemented such a system by having a system of workers continually handle cronjobs. The workers elect a leader that distributes jobs. The leader is chosen using the Paxos protocol
- (*Note: This was quite a good chapter to read after the Paxos chapter! Cron is a good example application that benefits from distributed consensus*)
- Many users want jobs that run once a day. To avoid that they all configure these jobs to run every day at midnight (the *thundering herd* problem), there is a special syntax that lets users specify that they do not care about the exact time. This let’s Google uniformly distribute the execution of these tasks over the course of a day
