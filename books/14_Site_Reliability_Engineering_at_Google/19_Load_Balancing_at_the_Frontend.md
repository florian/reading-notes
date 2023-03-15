## 19. Load Balancing at the Frontend

- This chapter is about load balancing between data centers. The next chapter is about load balancing between different data centers
- Even in a perfect world with perfectly reliable and powerful servers, we would want load balancing. The speed of light is a hard limit on how fast we can serve requests, so distributing requests to a close data center is important
- DNS load balancing
  - Before devices can send HTTP requests, they look up the IP address for the URL from a DNS directory
  - This allows us to route them to a certain data center
  - Configurations in the DNS directory are not very powerful. So typically one specifies a DNS middle server that then re-routes in a smart way. This requires one more round-trip, but allows much better load balancing
- Load balancing at the virtual IP (VIP) address
  - VIPs are not assigned to one particular network interface but can be shared across many devices
  - This allows adding and removing machines as necessary
  - A *network load balancer* receives packets and forwards them to one of the machines behind the VIP
  - For stateful protocols, the balancer needs to keep track of where it rounded the device previously to
  - This makes it much harder to do load balancing because one cannot randomly map to backend servers anymore
  - (*Note: Wow, suddenly I understand much better why stateless is so important in principles like REST*)
  - What if you add or remove servers (e.g. for maintenance)? *Consistent hashing* is helpful since it means that you need to reorganize much less of the hash table
  - (*Note: Consistent hashing was super interesting to read about!*)
- There are more ways of can use certain properties of DNS and VIPS to implement better load balancing
- > As with many things at scale, load balancing sounds simple on the surface—load balance early and load balance often—but the difficulty is in the details
