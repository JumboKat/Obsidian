### Why Coherence?
As an example, two cores each have a private write-back cache. If both of them store the same variable and one writes to it, the other's value becomes stale without it knowing, leading it to read the wrong value. A coherence protocol is needed to prevent this. Specifically, a coherence protocol enforces the following invariants:
- **Single Write Multiple Read (SWMR)**: at any moment, a cache line can have either one core writing to it or any number of cores reading from it.
- **Data value**: a read returns the value of the latest write that precedes it in that location's coherence order. Everyone agrees on the order of writes to the same address.
Usually, this is done via **invalidation**: Before a core writes, all other copies are invalidated. The standard protocol for this is **MESI**. 

Coherence tracks whole cache lines, not just variables (when any variable is read, its whole line is transferred).
### Coherence vs. Consistency
Coherence means that all processors agree on one order of writes to the same address (the **coherence order**) and eventually read the latest write in that order. This means two things:

