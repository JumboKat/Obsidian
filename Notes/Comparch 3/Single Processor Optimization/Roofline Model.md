The **roofline model** calculates the fastest speed a system can run at, and where the bottleneck is. 
### The Two Limits
A processor running a kernel has two independent limits: compute (FLOP/s) and memory bandwidth (B/s). The kernel needs W FLOPs and moves Q bytes. These two things happen in parallel, so whichever is slower is what sets the pace:$$t≥max(\frac{W}{P},\frac{Q}{BW})$$
- P is the peak compute rate in FLOP/s.
- BW is the peak memory bandwidth in B/s.
#### Example:
STREAM triad `a[i]=b[i]+s*c[i]` with $n=10^8$ doubles. The three arrays total 2.4GB, larger than cache, so every byte comes from DRAM:
P = 64 GFLOPS/s, BW = 20 GB/s
- $W=2n=2\times 10^8$ FLOP (one multiply, one add)
- $Q=(3\times 8)n=2.4\times 10^9$ B (2.4 GB) (read doubles b, c, write a, each double is 8 bytes)
- Compute limit: W/P = $\frac{2\times 20^8}{64\times 10^9}=3.1ms$
- Memory limit: Q/BW = $\frac{2.4 GB}{20}=120ms$
Memory sets the time. Making arithmetic faster does nothing, so we should decrease bytes or increase bandwidth.
### Arithmetic Intensity
**Arithmetic intensity** essentially means how much work is done for every byte brought in. It determines whether compute or memory wins:$$AI=\frac{W}{Q}$$Low AI points to a memory limit, while a high AI means a compute limit. AI depends on the algorithm and how well the caches work. The same matmul with 0.25AI can have 4 with blocking.
### Which Bytes Count?
![[Pasted image 20261117182636.png|379]]
The graph is a roofline model showing the predicted performance ceiling. The flat roof represents the compute peak P. The slope, representing $AI\times BW$, depends on which level in memory the bytes are coming from. Q only counts bytes that have actually crossed the DRAM link, after caches have done what they can. If the entire dataset fits in the cache, BW is based on the cache's access speed. Otherwise, old lines get evicted before they can be used again, so every access ends up coming from DRAM.
### The Model
![[Pasted image 20261117191505.png|377]]
The roofline model predicts the bound for performance using the following formula:
$$Perf≤min(P,AI\times BW)$$
- $AI\times BW$ is the slanted line. More work/line means more FLOPs as long as memory can keep up.
- P is the flatline; the core's peak.
The two meet at the ridge point, where AI = P/BW = 64/20 = 3.2 FLOP/B
- Left of the ridge: memory is the bottleneck.
- Right of the ridge: compute is the bottleneck.
### Optimizations
![[Pasted image 20261117191521.png|377]]
- Blocking, locality, and smaller data types reduce, moving the point right.
- Vectorization raises the ceiling.
- Prefetching helps a memory-bound point reach the slope, but doesn't change AI.
- Order matters; vectorizing the first does nothing since it is under the memory first. Block first, then vectorize.
### Limitations
The roofline model is still used today, but it has limits:
- It is an upper bound. A point far below it points to a different issue entirely (latency, dependencies, branch mispredictions).
-  Datasheet P and BW are rarely reached; measured ceilings are more accurate.
- A single core usually can't use the chip's full bandwidth, so use a single-core BW for single-threaded code.
- One kernel = one point; a whole program needs a profiler.