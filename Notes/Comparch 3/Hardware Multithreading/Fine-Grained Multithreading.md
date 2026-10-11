Also called **interleaved** or **barrel** multithreading, this multithreading method performs a context switch every cycle, giving each thread a fair chance to execute. 

![[Pasted image 20261010162540.png]]
Each stage in the pipeline holds an instruction from a different thread. The simplest issue policy is fixed **round-robin**; if a selected thread is stalled, send a nop.

If there are at least as many ready threads as pipeline stages, no two instructions of the same thread are ever in the pipeline together.
### Benefits
- **No thread-switch overhead**: we are simply fetching from a different thread every cycle. We do not need to save one thread's state, flush the pipeline, and load another's state to switch.
- **No pipeline interlocks/forwarding**: there are no register dependencies between instructions; since there is only one instruction per thread in the pipeline at any moment, so by the time one instruction belonging to a thread finishes, its results have been written and next instruction from the same thread will have what it needs to run.
- **No branch prediction**: Similar to the previous idea, an instruction calling for a branch will resolve by the time the next instruction runs.
- 