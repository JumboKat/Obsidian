![[Pasted image 20261117145526.png|392]]
C stores a 2D array row-by-row. A cache line of 64B holds 8 neighbouring doubles of one row. A row walk uses all 8 (one cold cache miss), while a column walk jumps a whole row (one miss each element). The standard solution to a **column walk** is a **loop interchange**, meaning swap the two loops. This results in 8x fewer misses.
### Other Standard Solutions
- **Fusion**: Merge two loops over the same array into one. Each element is used by both computations while it is still in the cache in lieu of being loaded twice.
- **Fission**: Split a loop in two to separate a dependence so that the other half can be [[Vectorization||vectorized]]. 
- **Unrolling**: Do multiple iterations per trip.
- **Invariant code motion**: Move a computation with the same result every iteration out of the loop.
- [[Cache Blocking (Tiling)||Tiling]]: Split loops into blocks that fit the cache.