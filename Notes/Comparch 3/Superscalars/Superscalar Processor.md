### In-Order Dual Issue
![[Pasted image 20261007211021.png]] The simplest form of a superscalar is an in-order dual issue (fetch/decode and issue two instructions). It contains two functional units (ALU/MEM and ALU2), allowing it to execute two operations at any given time.

Dual-issue (executing in the same cycle) is not possible when:
- A data dependence links the two instructions.
- A structural conflict (both need the same non-duplicated unit).
- The required functional unit is busy (multi-cycle operations or cache misses).

The problem here is **in-order** issuing: a stalled instruction means every instruction that comes after is stalled. Additionally, although instructions may be independent, the reuse of written registers creates false [[Dependencies||dependencies]] (WAR and WAW). 

To go faster, we need three things: branch prediction + speculation, register renaming, and dynamic (out-of-order) scheduling.
### Generic Out-of-Order Superscalar Processor
![[Pasted image 20261008173606.png]]
This is the generic design of a superscalar pipeline. 
- The [[The Front-End||front-end]] (instruction fetch, decode, and rename) occurs in-order. [[Register Renaming||Register renaming]] maps register names to fresh physical registers, avoiding WAR and WAW dependencies.
- The issue stage schedules instructions into a queue (based on dependencies, required FUs)
- Read registers, FUs + LS unit (operation execution) and reg write are the execution stages of an instruction. Instructions can be executed out of order here, based on [[Tomasulo's Method]].
- The commit stage outputs the results of each instruction in the order they were fetched. An instruction that finishes execution earlier must still wait for the previous instruction to commit before doing so.
### Modern Scheduler Architecture
![[Pasted image 20261009174555.png]]The modern scheduler architecture consists of:
- Register renaming (RAT + free list + DCL)
- Issue queues: each entry stores the opcode, tags (phys register IDs) of its sources, a ready bit per source, and destination tag. An entry is ready when both of its source ready bits are 1. There is an issue queue per FU class.
![[Pasted image 20261009175703.png|240]]
- A **unified physical register file (Unified PRF)** holds all the values in one place.
![[Pasted image 20261009180103.png|419]]
- When a FU is done its operation, the resulting tag is broadcast on the tag bus to all queues and its value is written to the unified PRF.
- Every entry waiting in the queue compares the tag to its source tags; if they match, the source's ready bit is set to 1.
- Among the ones waiting, select one to execute based on FUs available. Commonly, the oldest are selected first.
- Finished instructions are committed in-order via the ROB
#### Classic Tomasulo vs. Modern
Most of the core ideas of Tomasulo's Method are preserved, having just been modernized:
- Renaming is done via RAT + physical reg. file instead of via reservation-station tags.
- Operand values live in the unified PRF rather than in reservation stations.
- Only tags are broadcast, rather than the CDB broadcasting both the tag and result value.
- Instructions wait in issue queues rather than reservation stations.