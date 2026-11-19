### Why Coherence?
As an example, two cores each have a private write-back cache. If both of them store the same variable and one writes to it, the other's value becomes stale without it knowing, leading it to read the wrong value. A coherence protocol is needed to prevent this. Specifically, a coherence protocol enforces the following invariants:
- **Single Write Multiple Read (SWMR)**: at any moment, a cache line can have either one core writing to it or any number of cores reading from it.
- **Data value**: a read returns the value of the latest write that precedes it in that location's coherence order. Everyone agrees on the order of writes to the same address.
Usually, this is done via **invalidation**: Before a core writes, all other copies are invalidated. The standard protocol for this is **MESI**. 

Coherence tracks whole cache lines, not just variables (when any variable is read, its whole line is transferred).
### Coherence vs. Consistency
Coherence means that all processors agree on one order of writes to the same address (the **coherence order**) and eventually read the latest write in that order. This requires two things:
- **Write propagation**: a write is visible to all other processors.
- **Write serialization**: all processors sees the same order of writes to one location. No two writes happen in parallel.

Consistency applies to all memory locations. It deals with the problem: in what order may accesses to different addresses appear to other processors?
### Snooping
In an architecture that implements **snooping**, all cache controllers snoop the bus and is able to see every transaction on the bus. It checks if a write pertains to an address that it holds. Each cache has a small state machine, that stores the state of each of its cache lines as tags and responds to requests on the bus. The snooper needs its own copy of tags (**snoop tags**) so that it does not steal cycles from the core.
#### Processor Requests and Bus Transactions
The processor can send the following signals to its cache controller:
- **PrRd**: the processor requests a read from the core. On a hit, it is serviced by the cache and nothing reaches the bus. On a miss, the core must send a BusRd request to fetch the data.
- **PrWr**: the processor writes to the cache. On a hit, if the cache line is tagged M or E, no further requests. If the cache line is marked shared (S), send a BusUpgr signal. On a miss, signal BusRdX (BusWr if write-through)

The following are signals on the bus put on by cache controllers and seen by every snooper:
- **BusRd**: issued when a PrRd leads to a miss. If another processor has the same line tagged M, it flushes it to the bus, and updates the requester and all others holding the line. The line now becomes S.
- **BusRdX**: occurs on a write miss. Get a copy of the value and sole ownership. All other copies are invalidated. An M holder supplies the value first.
- **BusUpgr**: occurs when writing to a line that is shared (write hit). Still invalidate all others.
- **Flush**: occurs when replying to a snoop or evicting a dirty line. Sends the data out to the bus, received by memory and the requester.
- **BusWr**: issued on a store with write-through protocol. All other copies are invalidated.
### Snooping Cache Controller
![[Pasted image 20261118202448.png|524]]
The cache controller includes:
- **Data buffer**: incoming data is buffered and sent to the cache when it is not busy.
- **Write-back buffer**: outgoing data is buffered until the data bus is free.
- **CPU-side controller**: receives addresses/commands.
- **Bus-side controller (snooper)**: connected to the snoop bus, listens for bus transactions.
- The cache contains two sets of control + state tags: one for processor and one for the snooper. This lets the processor communicate with the cache and the cache communicate with the bus/snooper simultaneously.
### Coherence Protocols
#### Valid-Invalid
![[Pasted image 20261118200904.png|381]]
The simplest form of protocol. Implements write-through with invalidation. Every cache line has two possible states: **Valid** and **Invalid**. Every write goes on the bus and to memory (write-through). Other caches that see BusWr for a line they hold will invalidate it. Their next read misses and must read from memory. Memory is always made up-to-date, however every store uses bus bandwidth.
- Ex: for 8 cores, each store per 10 instructions at 2 GIPS, 8 B per store: $8\times 0.2\ G\times 8\ B=12.8GB/s$ of write traffic alone.
This protocol can only work with write-through; with write-back, there is no way to know when data becomes stale. This protocol would need a third state.
#### MSI
![[Pasted image 20261118201602.png|476]]
**MSI** protocol has three states:
- **M**odified: the cache line has been modified (dirty) and is the only valid copy; memory holds a stale copy.
- **S**hared: the line is clean, and other caches may also hold it.
- **I**nvalid: line is stale/invalid, the next read is a miss.

- When a cache reads a line that is S, the sharer flushes it to the bus and to memory. If there is an M holder, it flushes it, and the line becomes S for every holder. 
- When a holder writes to a line that is already S, its cache sends BusUpgr and invalidates all other copies, making its own copy M. If it was initially I, it would send a BusRdX instead of BusUpgr.
#### MESI
![[Pasted image 20261118204043.png]]
The **MESI** protocol adds an **exclusive** state E. A cache line that is the sole copy of its line. This means a write to it does not require a bus transaction, since there's nothing to invalidate; E transitions to M silently.  On a read miss, if the cache line isn't S in another cache, it becomes E in the reading cache.

The E state costs an extra bus line (a read miss can either go from I to S or E) and requires a more complex controller. It also must wait for all snoopers before choosing between S or E on a write miss (must check if others are sharing). This was not a problem in MSI, where a miss always went to S. Its inclusion does however save on bus transactions on every write by half, and the costs are relatively small.
#### MOESI and MESIF
**MOESI** adds the **O**wned state, meaning a line is dirty and shared. When another core reads an M line, the owner goes from M to O and sends the data cache-to-cache without updating the memory. The owner writes back later. Used by AMD, it is useful when memory is slow/far away. MOESI is the general implementation, while MSI and MESI are subsets of it.

**MESIF** is used by Intel and adds the **F**orward state. An F line is marked the sole clean sharer and flushes whenever a read is requested, ensuring there are no duplicate responses.
#### Invalidate vs. Update Protocols
![[Pasted image 20261118205338.png|503]]
With invalidate, a write removes other copies; later readers miss. With update, a write sends the new word to all other sharers, guaranteeing read hits. 

A **write run** is the number of consecutive writes by one processor to a line before another processor access it. 
- Ex: R1 W1 R1 W1 W2 R2 has a run of 2.

The protocol that wins depends on the expected size of write runs, as the graph shows. With update, a long write run means several unnecessary updates (they aren't being used) as opposed to invalidate, which incurs a miss, but only at the end of a string of writes. Since long writes are common (a core updates its own data repeatedly), all mainstream CPUs use invalidation.
### Coherence Misses
The fourth C in cache miss types. A **coherence miss** occurs when a processor reads a line made invalid by another processor. There are two types:
- **True sharing miss**: the same word being accessed was written by another processor since it was last accessed; essential.
- **False sharing miss**: the same *line* holding the word being accessed was invalidate because of a write to a different word; non-essential.
Larger lines means fewer cold misses but more false sharing misses.
#### Example
Two processors A and B. Each has a direct-mapped cache with 64 blocks of 16 byte lines. All lines are initially invalid. Memory initially all zeros.
![[Pasted image 20261118211333.png]]
16 byte blocks means 4 offset bits; 64 blocks means 6 index bits. This leaves two bits for the tag.
 The two addresses 1200 and 1208 have the same index, meaning they occur on the same line. The entire line spans addresses 1200, 1204, 1208, 1212. Given the 