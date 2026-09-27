A TCM is a fast on-chip SRAM that connects directly to the core and is mapped at a fixed address range. Accessing it occurs in a single cycle, and it is impossible to miss.

![[Pasted image 20260926183517.png]]
It is not a cache; it is ordinary addressable memory that is very fast and very close to the core. It is typically split into an instruction fetch component (ATCM) and data access component (BTCM). Each core has its own private TCM; no coherence protocols are required.
### TCM vs Cache
Unlike a cache, where hardware decides its contents, the contents of the TCM is explicitly allocated by the programmer (software). The cache is fast on hit, slow on miss (variable), while accessing a TCM is fixed and single-cycle. The cache is best for larger, unpredictable working sets; the TCM is for a small portion of time-critical code/data.
### Accessing the TCM
