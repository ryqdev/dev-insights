# Performance Test
https://www.digitalocean.com/resources/articles/cloud-performance-testing


## Thinking
If the cloud system can autoscale, will it make sense in stress testing?

For CPU and memory perspective, it probably does not make sense.

However, in the real world distributed system, many problems occur in:
- DB, cache, network, storage will be the bottleneck 
- Even with autoscaling, new pods cannot start fast enough to lighten the system load