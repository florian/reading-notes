# Part II – Principles

## 3. Embracing Risk

- Purely optimizing for zero downtime is not a great idea
    - Users will not notice the difference between an uptime of 99.99% and 99.999%
    - Other less reliable components (such as the cellular network) will still lead to the service not being available all the time
    - Furthermore, optimizing for zero downtime comes at a cost of slowing down feature development
- Reducing downtime further does not have linear cost, but rather exponential ones since
    - the cost of the necessary, redundant machines quickly builds up
    - there is a high opportunity cost of not being able to launch as many things because you need to keep the service reliable
- > We strive to make a service reliable enough, but no more reliable than it needs to be
- Measures of availability
    - time-based: what percent of the time is the service available. At Google-scale, this does not work well anymore because it is incredibly rare that something fails across all timezones and regions at the same time
    - aggregate availability: what portion of requests are successful. The disadvantage here is that not all requests have the same importance
- Reliability features of services
    - Different services have different requirements. When Google acquired YouTube, it decided to set much looser targets, allowing for more iteration speed when it comes to features. Products targeting enterprise customers have to be much more reliable than that though
    - Services may fail differently. For some applications it could be fine to not serve some images but to still have the general service available. Other services have natural down hours where few customers use the service and maintenance can be performed
    - Some services allow setting maximum failure rates in very quantitative ways. For example, for ads served one can compute quite well how much money is lost by missing reliability vs. how much that extra reliability costs
    - Batch jobs generally care more about throughput than availability
- Error budgets
    - Product management sets a SLO (e.g. desired uptime) for the service
    - Each quarter, it is tracked how close the service comes to achieving that objective
    - Once the SLO is not met, releases for the quarter are paused
    - This creates a feedback loop that ensures that product teams ship things that work reliably enough. If they do not, then they eventually have to fix things before working on new features
