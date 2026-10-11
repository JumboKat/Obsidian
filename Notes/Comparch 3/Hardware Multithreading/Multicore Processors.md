![[Pasted image 20261010155347.png|421]]
Having multiple cores is the most generic way to exploit coarse-grained parallelism. We simply add another core and add shared caches with coherence. However, the cost is significant; we are doubling the on-chip area and power dissipation. Additionally, adding more cores does not necessarily increase efficiency; a single out-of-order core is rarely 100% busy; cache misses, branch mispredictions, and dependencies causes a lot of idle wait time.

What we want is greater utilization on a single core.