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
**Arithmetic intensity** essentially means how much work is done for every byte brought in. It determines whether compute or memory 