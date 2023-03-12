## 4. Service Level Objectives

- Service level (SL) terminology
    - Indicators (SLI): A quantitative metric that can be tracked, for example latency, throughput, or error rate
    - Objectives (SLO): How you would like this metric to be, e.g. that it should not surpass some upper bound
    - Agreements (SLA): The consequences when the objective is not meant. Usually this is determined by product or business units
- Availability is often described by counting the 9s
    - 99% → “two nines”
    - 99.999% → "five nines”
    - 99.95% → “three and a half nines” (Google Compute Engine's target)
- Durability: For how long data can be retained by a system
- Lessons learned from setting objectives at Google
    - Google makes Chubby (its globally distributed locking mechanism) fail in some small amount of cases so that developers do not blindly depend on it always being available
    - It is helpful to think of metrics as distributions, not as averages. The average can hide a lot of information, e.g. whether traffic spikes at certain points in time or whether latency is much slower for some users
    - Some SRE teams look at the 99.9th percentile of users. Satisfying constraints for this percentile means that the typical user experience is probably fairly good
    - SLO safety margin: It can be a good idea to have a tighter internal SLO to make it really unlikely to cross the public SLO
