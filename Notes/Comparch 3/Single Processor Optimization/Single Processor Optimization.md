![[Pasted image 20261115182910.png]]
A single processor SIMD core loses time and energy in several different areas. By default, lower level memory accesses become costlier by about 100x from L1 to DRAM. The problems causing time and energy loss are as follows:
- Too many instructions, poor code layout: occurs in the core, fixed by [[Compiler Optimization||compiler optimization levels]].
- Idle lanes, only one is doing work: occurs in the core, fixed by [[vectorization]].
- Wrong access order: L1 cache, fixed by [[loop transformations]].
- Working set is larger than the cache: L2, fixed by [[cache blocking]].
- core stalls waiting on misses: LLC, fixed by [[prefetching]].
- Bandwidth (bytes/s) limit: DRAM, fixed by [[bandwidth techniques]].
The [[Roofline model]] helps determine which technique is necessary.