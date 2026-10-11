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
- Repeat and report variance: measure many times, report median and spread.
- Isolate the region: measure kernel, not whole program's start-up and I/O (unless that is the point).
- Beware the observer effect
- Change one thing at a time and re-measure to check.
### Heterogenous SoC (System on Chip)
A SoC is comprised of a CPU and some hardware accelerators that all share cache and memory. They can each perform compute tasks on their own, but contend for the same memory access. This SoC contains three engines: a CPU, GPU, and NPU.
![[Pasted image 20260925161057.png]]

Latency includes not only compute time, but data movement between engines and launch/synchronization overhead. These cannot be avoided and during these times, no engine is computing.
![[Pasted image 20260925161405.png]]

We can pipeline across different stages of a task to the different engines as shown below to reduce stalling. Throughput is determined by the slowest single stage, rather than the sum of all individual stages plus copies.
![[Pasted image 20260925161418.png]]
#### The Transfer Tax
Offloading a stage to an accelerator incurs an overhead cost of moving the data back and forth. According to Amdahl's law, it is only worth it if the accelerator time and transfer time is less than the time taken simply computing on CPU:
$$\frac{T}{s}+T_x<T⇐⇒ T_x<T(1-\frac{1}{s})$$
Where:
- T = Time it takes to execute the stage on CPU.
- T<sub>x</sub> = Time it takes to transfer the data to and from accelerator.
- s = Speedup gained by executing on accelerator.