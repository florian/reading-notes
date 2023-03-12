## 10. Practical Alerting

- White-box monitoring: Borgmon
    - Borgmin is a tool that collects logs from various servers
    - A Borgmon can collect from other Borgmons
    - It has a schemaless textual interface for describing what to aggregate. This makes it easy to add new things
    - Borgmon stores the data in a way that allows for easy slicing, and makes it easy to run various queries against it
    - > Borgmon is really just a programmable calculator, with some syntactic sugar that enables it to generate alerts
    - Borgmon configs separate what is logged and how it is aggregated
- Black-box monitoring: Prober
    - While white-box monitoring is really helpful for understanding lots of things, it misses some signals. If users never get to the server in the first place because of DNS misconfigurations, white-box monitoring cannot observe this. Instead, you need a white-box monitoring tool
    - Prober is a tool that tries to load resources like a user would, and then reports back various metrics
    - It can expose the results in a way that they can also be read using Borgmon
- > Ensuring that the cost of maintenance scales sublinearly with the size of the service is key to making monitoring (and all sustaining operations work) maintainable. This theme recurs in all SRE work, as SREs work to scale all aspects of their work to the global scale.
