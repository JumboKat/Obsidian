The **roofline model** calculates the fastest speed a system can run at, and where the bottleneck is. 

A processor running a kernel has two independent limits: compute (FLOP/s) and memory bandwidth (B/s). The kernel needs W FLOPs and moves Q bytes. These two things happen in parallel, so whichever is slower is what sets the pace:$$t≥max(\frac{W}{P},\frac{Q}{BW})$$
- P is the peak compute rate in FLOP/s.
- BW is the peak memory bandwidth in B/s.
#### Example:
STREAM triad `a[i]=b[i]+s*c[i]` with $n=10^8$ doubles:
- $W=2n=2\times 10^8 FLOP$ (one multiply, one add)