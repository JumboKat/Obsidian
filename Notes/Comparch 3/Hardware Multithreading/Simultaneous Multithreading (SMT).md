 [[Coarse-Grained Multithreading||Coarse-]] and [[Fine-Grained Multithreading||fine-grained]] MT are forms of **temporal** multithreading. In any cycle, each pipeline stage holds instructions from one thread, meaning the core is shared across time. On superscalar cores, this leaves horizontal waste. some issue slots/functional units are idle during low IPC phases.

**Simultaneous Multithreading** allows instructions from different threads to occupy the same stage in the same cycle. SMT turns thread-level parallelism into instruction-level parallelism for an out-of-order machine.
### SMT Core
![[Pasted image 20261011193051.png]]
Different parts of an SMT system are shared to different degrees:
- Instruction fetch, decode, rename, commit and writeback are time-shared (one instruction at a time).
- Since the OS sees each hw thread as a separate logical CPU, we must duplicate structures responsible for the thread's state for each thread.
- Structures that preserve program order: fetch/decode queues, ROB, and load/store queues are partitioned, giving each thread its own space. This also prevents one starved thread from filling the whole structure and starving the others.
- PRF, schedulers (reservation stations), FUs, caches, and predictors are all shared. Renaming allows for PRF sharing to work; each thread has its own RAT, but all maps point into one common pool of physical registers.
- Extra logic: we must have a fetch-selection policy to determine which thread is fetched each cycle, and thread IDs carried through the whole pipeline so that results, flushes, and commits go to the right thread.
![[Pasted image 20261011193944.png]]
SMT focuses on maximizing utilization in FUs.
### Fetch and Issue Policies
#### Fetch Policies
- Fetch from one thread per cycle (RR or priority).
	- To fetch several per cycle, we would need multi-ported/ banked I-cache access.
- ICOUNT: favour the thread with the fewest instructions in flight; it is making progress and not clogging up queues.
- Stall or flush a thread on an LLC miss so it does not hog shared queues.
#### Issue Policies
- Issue from all threads each cycle; skip threads with no ready instructions.
- Prioritize oldest first.
- Low priority to speculative instructions (instructions that execute conditionally, i.e. a branch or if statement. We fetch both possible instructions and flush the one that is not chosen).

Fetch policy is more important than issue policy; a bad fetch policy can fill shared queues with stalled instructions.
### Downsides
SMT comes with the following downsides:
- Single-thread performance may drop as threads contend for shared resources. Extra logic can also slow clock frequency.
- Increased complexity, with thread IDs, TLB and predictor tagging, more complicated design, as well as increased test and verification time.
- The load/store queue needs to respect memory consistency between sibling threads.
- Performance isolation: no priority for real-time instructions.
- 5-15% increased area consumption.