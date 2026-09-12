In terms of performance, architectures can fall into one of four categories based on the number of data and instruction streams: single instruction single data (**SISD**, which includes **RISC** processors), single instruction multiple data (**SIMD**), multiple instruction single data (**MISD**), multiple instruction multiple data (**MIMD**).
### Parallelism within One Processor/Core
#### Pipelining
Instructions overlap in different stages (IF, ID, EX, etc.). Achieves temporal *instruction-level parallelism* (**ILP**), with different instructions at different pipeline stages.
#### Multiple Issue
Issues more than one instruction per clock cycle, with individual units processing different instructions.
- **Superscalar**: has an instruction scheduler that dynamically assigns instructions at runtime.
- **VLIW** (*Very Long Instruction Word*): Compiler decides the instructions to be run in parallel. Must be recompiled on new systems, which makes superscalars more mainstream.
#### Vector / SIMD
One instruction is processed by multiple units at a time via data parallelism. Vector extensions work in the same way.

A vector instruction takes two vectors (fixed-size groups of values in a wide register) and produces a vector result: \[a0, a1, a2, a3] + \[b0, b1, b2, b3] = \[c0, c1, c2, c3],
whereas a scalar instruction takes two single values and produces a single result (a + b = c).

A vector extension refers to additions in a processor's ISA that allow it to operate on multiple data elements within a single instruction.
#### Core Multithreading
This refers to thread-level parallelism: running multiple threads on the same core.
- **Fine-grained**: switches threads every cycle.
- **Coarse-grained**: switches threads only on a long stall (e.g. cache miss)
- **SMT** (*Simultaneous Multithreading*): instructions from multiple threads are issued in the same cycle to different functional units; the processor is at full execution capacity.
#### Matrix / Tensor Units
Use complicated, specialized processing units to perform many **MAC** operations (*Multiply and Accumulate*) under a single instruction.
### Multiprocessor Memory Architectures
#### Uniform Memory Access (UMA)
In UMA, all processors access memory via a shared bus/network. The memory, which can be one central memory or several memory banks, can be accessed from any processor within an equal amount of time.
#### Non-Uniform Memory Accessm (NUMA)
![[Pasted image 20260912112133.png|287]]
In NUMA, processors have access to their own memory, which are then connected to a shared network. A processor accesses its own memory in the shortest amount of time; if it wants to access another processor's memory, it must first go through the network, which takes much longer.
#### Cache-Only Memory Architecture (COMA)
In COMA, there is no main memory; only caches. While processors in all memory architectures can have a cache, in COMA, memory ONLY exists within caches; there are no permanent copies of data in memory. When a processor accesses data, that data migrates into the local cache of the node using it; the data lives wherever it is currently used. COMA is similar in design to NUMA, except that it uses hardware cache-coherence protocols to ensure consistency among caches.