## 20. Load Balancing in the Datacenter

- A simple load balancing strategy in the data center would be that a load balancer keeps track of how many requests are queued for a worker, and then stops distributing to the ones that have too many open ones
- That strategy is not effective in practice since it is hard to configure a limit for how many requests a worker should take care of without it exhausting its resources
- A more interactive protocol is more robust. Workers tell the balancer whether they are busy or can take up more work. The balancer distributes based on these signals
- *Subsetting*
  - The balancer and workers keep connections open because it's cheaper than to re-establish them all the time
  - To avoid too many open connections at a given time, it is useful to make sure that each client can only reach a small subset of backend workers
  - Randomly deciding on that subset leads to unevenly distributed machines. Instead, it is better to pick the machines in a smarter way
  - (*Note: I could not understand why randomly choosing leads to uneven distribution. I don't think the chapter is very clear on why that is*)
- Load balancing policies
  - *Round robin*: This only works well in homogeneous situations. If some requests are much more expensive or some machines have much less power, it leads to unevenly distributed work. Google created the concept of *Google Compute Units* (*GCU*) to determine how powerful a machine is because of this problem
  - *Least-loaded round robin*: You assign to the worker with the least load. Estimating the load is however not that easy
  - *Weighted round robin*: The coordinator keeps track of how well workers have done so far (in terms of success / error rate) and assigns with a weight based on this score
