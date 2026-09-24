Performance models like AMAT assume predictable inputs and conditions. The model does not account for prefetchers, speculation, contention, nor OS-level effects (scheduling, paging, interrupts). Thus, you need to **profile** — actually measure the system running. There are some distinctions to be aware of when doing so:
- **Wall-clock time vs. CPU time**: wall-clock time is the tot-al elapsed real-world time, including waiting times (I/O, network, other engines), while CPU time only counts the time spent by the processor during execution. In heterogenous applications (multiple engines, like CPU + GPU), wall-clock time is what actually matters as this is what the user experiences.
- **Throughput vs. latency**: throughput measures how many requests are completed per unit of time, while latency measures how long it takes for a request to be serviced from start to finish. These are not always aligned, and you should optimize the one the user feels the most (e.g. batch job cares about throughput, an interactive UI cares about latency).
- Where time goes matters more than the total; breaking down which parts take time lets us know where to focus optimization efforts.
- Measure before you optimize, then measure again to confirm the changes actually helped.
### Hardware Performance Counters
Every core has a **Performance Monitoring Unit (PMU)** that contains hardware counters that count events, including:
-  Instructions per cycle (IPC)
- Cache-misses, cache-references
- Branch-misses
- Mem-loads, mem-stores
- data TLB-load-misses, instruction TLB-load-misses

The PMU can operate in two modes:
- **Counting** mode: checks the total count at the end of the program (the *performance stat*).
- **Sampling** mode: periodically checks counts based on events (e.g. every 10000 instructions, the *performance record*). This is carried out by calling the ISA, which interrupts execution.
### Measurement Methodology
- Warm up first: the first run pays cold caches, TLB, page faults, and CPU frequency ramp.
- Repeat and report variance: 
- Isolate the region: