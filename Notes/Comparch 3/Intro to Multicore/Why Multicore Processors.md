### The Single Core Limit
![[Pasted image 20261117200230.png]]
The industry moved to multicore as single core hit physical limits:
- **Dennard Scaling**: as transistors shrank, voltage could drop too, so power density stayed constant. Smaller transistors meant faster chips at the same power. This ended around 2005, where leakage current prevented lowering voltage any further.
- **Power wall**: with voltage V constant, raising frequency f raises voltage, which raises power comsumption by a power of two ($P_{dyn}=\alpha CV^2f$)
- **ILP Wall**: wider OoO cores give diminishing returns.
- **Memory wall**: DRAM latency can't keep up with processor speed
- **Moore's law** meant more transistors on a chip, so the only use for them was more cores.
#### Example
![[Pasted image 20261117195651.png]]
Given the baseline of 1 core at power 1 and throughput 1. 
- For a faster single core at 1.25f, you would need 1.25V, meaning $P=1.25^3=1.95$ for ≤ 1.25x the speed.
- With two cores at (0.8V, 0.8f):$$P=2\times 0.8^3=1.02 \\ throughput =2\times 0.8 = 1.6$$
Two cores deliver 60% more throughput for roughly the same amount of power as the baseline.

**Pollack's Rule**: single-core performance scales with roughly the square root of area. So, spending 2x the area (transistors) on a bigger core only gives 1.41x the performance, whereas spending it on two cores gives 2x the performance.
### Multicore Hierarchy
![[Pasted image 20261117202505.png]]
- **Chip multiprocessor (CMP)**: several cores with a shared cache (usually) on one chip or package.
- **Symmetric multiprocessor (SMP)**: several processors with equal access to one memory.
- **cc-NUMA server**: several sockets, each with its own memory but one shared address space.
- **Cluster**: many servers, each a shared-memory machine, connected by a network. Message passing between nodes, shared memory within nodes.
### Cache Organizations
![[Pasted image 20261117202515.png]]
#### LLC: Private vs Shared
There are two ways to organize the LLC: either give each core its own slice (private), or let all cores share one big pool.
##### Private LLC
- Low and uniform hit latency; cache is right by the core.
- Capacity is fixed at slice size (C/N; total capacity / number of cores).
- Copies of shared data is replicated in each cache, wasting space.
- No competition for capacity, but memory BW is still shared.
- Cache-to-cache transfers via coherence protocol.
##### Shared LLC
- Higher hit latency and depends on slice distance (a shared LLC is split into slices across the chip).
- Each core has access to the entire cache's capacity.
- There only exists one copy of a shared data.
- Competition between cores; one thread can evict another's data.
- Communication occurs through interconnects, coordinated by the LLC/home agent.
##### Example
Four cores, 8 MB of LLC. Thread A has a 6 MB working set, B-D need 0.5 MB each.
- Private: each core gets exactly 2 MB. A needs 6 MB, so it misses constantly. B, C, and D only use 0.5 MB, with the rest being wasted.
- Shared: The total capacity accommodates all four cores. Capacity is allocated to whoever needs it.
If one core B starts streaming through 1 GB of data (reading/writing through a large block of data without reusing it). This is **cache pollution**: B never reuses the data, but cache lines belonging to other core's get evicted.

The modern solution is to share, but partition when needed. E.g. assign cache ways to applications.
#### Interconnects and Directories
![[Pasted image 20261118175755.png|242]]
- With 2-8 cores, a **bus** is serviceable. 
- For 8-12 cores, a ring network and LLC slices are appropriate.
- Any more than that, a 2D mesh of tiles, or chiplets around an I/O die.
- The LLC is sliced; a fixed address hash picks the home slice of each line, so lookups spread over all slices and cache bandwidth grows with the core count. This also means every memory access has a distance depending on which slice/chiplet/socket its located in.
#### Shared-Memory Abstraction
Despite it being sliced, all threads see memory as one address space. Communication does not need to be made explicit. This comes with three requirements:
- **Coherence**
- **Consistency**: rules for the order in which accesses to different locations become visible.
- **Synchronization**: atomic operations to build locks and barriers.

Private cores complicate shared memory:
- **Coherence**: two cores can hold different values for the same address; requires protocols such as MESI.
- **Consistency**: in what order do different addresses become available?
- **Scalability**: broadcasting every write to 200 cores does not scale; solved by [[networks]] and [[interconnects]].
#### UMA vs. NUMA
![[Pasted image 20261118180739.png]]
NUMA is the modern standard for connecting cores to memory:
- Every multi-socket system is NUMA, since each socket has its own memory controller. It is also still **cache-coherent (cc-NUMA)**. Caches are consistent across nodes, so software still sees one share address space. Only performance differs.
- Large sockets are internally also NUMA.
- **First-touch policy**: a common Linux default policy places a newly allocated page on the first node of the thread that first touches it.
#### Chiplets
A **die** is a single block of silicon cut from a wafer with a complete circuit on it. Chip factories print hundreds of identical copies of a design onto one big silicon wafer, then cut it into individual dies. A chip is a die (or several) mounted in a package with pins or pads. A **chiplet** is a small die meant to be combine with others in one package, rather than being the whole processor itself. So why build one processor out of several small dies rather than one big one?

- **Yield**: manufacturing defects can land randomly on a wafer, usually runs the die it lands on. The bigger a die is, the more likely it is defective. Poisson's simplified model states:
$$Y=e^{-DA}$$
- Y = yield, the fraction of dies that come out without defects.
- D = defect density (defects/$cm^2$)
- A = die area ($cm^2$)
Yield falls exponentially with area; doubling the area hurts more than doubling the defect count.

- 
 