### Instruction Level Parallelism
![[Pasted image 20261007204839.png]]
A scalar pipeline holds at most one instruction per stage. Hazards (branches, cache misses, dependencies) push it well below 1 per cycle. **ILP** means processing multiple instructions per cycle. 
### ILP Solutions
There are two ways to exploit ILP:
- **Superpipelining**: cut each stage into M sub-stages, clock M times faster.
- **Superscalar**: replicate processor resources (multiple functional units). We can fetch/decode/execute P instructions per cycle.

In practice, superscalar is what is used, since clock frequency has hard physical limits.
![[Pasted image 20261007205800.png]]
