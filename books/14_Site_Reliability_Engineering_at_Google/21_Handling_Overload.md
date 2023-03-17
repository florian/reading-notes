## 21. Handling Overload

- Load balancing is important and useful but sometimes you cannot prevent overload from happening
- In such cases, it is important to degrade gracefully, e.g. by showing approximate results
- At Google, services have a quota of how many requests they can handle. If the quota is surpassed, requests are rejected
- However, even rejecting requests can be quite expensive if there are too many of them. To overcome this, Google adds client-side throttling of requests if the client notices that too many requests are failing
- Requests are annotated with how critical they are. When the service is over it's quota, the less critical requests get dropped first
- There is a protocol for re-trying requests if they fail. Clients re-try a few times before bubbling up the error to the caller. If they re-try for too many requests, the re-tries get throttled
- (*Note: Reading through this chapter, it feels like there are a lot of heuristics involved, e.g. to decide how often a given request can be re-tried or how often a client can re-try in total*)
- In large-scale RPC systems, it is important to consider the costs of keeping too many RPC connections alive. These require periodic check-in calls, so the costs can add up
- This quote seems to summarize the chapter neatly:
  > a backend task provisioned to serve a certain traffic rate should continue to serve traffic at that rate without any significant impact on latency, regardless of how much excess traffic is thrown at the task
