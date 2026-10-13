#ComparchIII
Every mainstream computer is a parallel machine.
### Before Parallel Processing
For 30 years, computers got faster by raising the clock frequency and widening [[Instruction Level Parallelism||ILP]]. This occurred until 2004-2006. Dennard scaling no longer applied; shrinking transistors no long led to proportionally lower power consumption. Additionally, clocks plateaued near 3-5 GHz because of the **power wall**. We also ran into diminishing single-thread ILP returns (**ILP wall**) and memory latency lagged (**memory wall**).

The industry's answer was to put many cores on one chip, add wide data-parallel units, and offload work to specialized accelerators. Performance improvements now come from parallelism, not frequency.
### Types of Parallelism
#### Inside One Core
- **ILP** - pipelining, superscalar, out-of-order execution.
- **DLP** - one instruction, many data ([[SIMD in General-Purpose CPUs||SIMD]]/[[Vector Processors]])
- [[Simultaneous Multithreading (SMT)]] - one core runs several hardware threads.
#### Across the Chip
- **TLP**: many cores executing independent threads.
- **Heterogeneity**: fast and efficient cores.
- **Accelerators**: GPU (graphics/compute), NPU (AI)
### Chip Designs
#### Intel Core i7
![[Pasted image 20261012212944.png|488]]
The Intel Core i7 is an asymmetric/hybrid multicore, boasting:
- six **P-cores**: wide, out-of-order, high clock, focus on latency.
- two clusters of four **E-cores**: small, area/energy-efficient; focus on throughput.
- An Intel Thread Director reports each thread's expected P vs E benefit to the OS scheduler.

Each P-core holds 2 threads, 12 in total. All cores share one memory address space. The chip is a shared-memory MIMD model.