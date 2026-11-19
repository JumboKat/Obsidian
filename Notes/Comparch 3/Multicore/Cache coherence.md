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
- 
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