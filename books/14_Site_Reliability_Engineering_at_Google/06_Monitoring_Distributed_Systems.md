## 6. Monitoring Distributed Systems

- Monitoring
    - *White-box*: Use metrics exposed in internal systems
    - *Black-box*: Query the service yourself like a user would
    - Google performs a mix of these. Black-box monitoring is helpful because it allows for high alerts when users are definitely impacted. White-box monitoring can provide earlier and more fine-grained signals, which might not yet lead to a user impact
- When looking at failures, it is important to distinguish between symptom (e.g. what is down?) from cause (e.g. why is it down?)
- The four golden signals
    - > If you can only measure four metrics of your user-facing system, focus on these four
    - Latency: how long does it take to satisfy requests, best tracked across successful and failing requests
    - Traffic: how much demand is placed on the system
    - Errors: rate of requests that fail
    - Saturation: how much of your resources is currently used (e.g. in terms of memory)
- Best practices for alerts
    - Does this detect something otherwise undetected?
    - Is there a clear action to be taken associated with the alert?
    - Is it clear whether this affects users or not?
