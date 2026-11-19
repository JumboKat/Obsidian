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
- **BusRd**: issued when a PrRd leads to a miss. If another processor has the same line tagged M, it flushes it to the bus, and updates the requester and all ot