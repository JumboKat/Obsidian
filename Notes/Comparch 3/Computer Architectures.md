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
#### No Remote Memory Access (NORMA)
In NORMA, processors cannot access the memory of other processors; they must send and receive data messages.
### Multiprocessor Cache Organization and Coherence
![[Pasted image 20260912175615.png]]
**Coherence** is the maintenance of no stale data; if a value exists in two or more caches and is changed in one, the system must know that the same value in the other cache(s) is invalid. There are three ways in which caches are organized and coherence is maintained:
- Private: caches are separate but can communicate coherence with each other.
- Shared: all processors access the same cache, so coherence protocols are not necessary.
- Hybrid: the most commonly adopted, which combines elements of private and shared organizations. Each processor has its own private L1/L2 cache, and each cache communicates to a shared last-level cache.
### Interconnection-Network Architectures
![[Pasted image 20260912180238.png]]
In other words, how processors/nodes in a parallel system connect with each other.
#### Direct / Static
Every switch/router in the network is attached to a processing node (endpoint). Processors communicate through neighboring routers. Possible configurations include:
- **Ring**: nodes are connected in a circle, each with two neighbors. Simple and cheap but inefficient as the number of nodes increases.
- **Mesh/torus**: nodes are arranged in a grid of neighbors; a torus adds wraparound links to reduce worst-case distance.
- **Hypercube**: each node has a neighbor for every bit position in its address. Low diameter (log of the number of nodes) but increased wiring complexity per node as the system scales.
#### Shared Medium
All nodes connect to a common bus and take turns accessing it. This is the simplest design but scales poorly as only one can access the bus at a time. Typically for small-scale systems.
#### Indirect / Dynamic
A grid with switches that route traffic but aren't themselves processing nodes. The nodes sit on their own grid lines (rows and columns), and the intersections of any two lines are a switch that can be activated when communication is required.