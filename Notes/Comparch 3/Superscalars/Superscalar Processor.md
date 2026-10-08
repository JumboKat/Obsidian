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
- The instruction fetch, decode, and rename occur in-order. Renaming maps register names to fresh physical registers, avoiding WAR and WAW dependencies.
- 