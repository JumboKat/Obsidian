### In-Order Dual Issue
![[Pasted image 20261007211021.png]] The simplest form of a superscalar is an in-order dual issue (fetch/decode and issue two instructions). It contains two functional units (ALU/MEM and ALU2), allowing it to execute two operations at any given time.

Dual-issue (executing in the same cycle) is not possible when:
- A data dependence links the two instructions.
- A structural conflict (both need the same non-duplicated unit).
- The required functional unit is busy (multi-cycle operations or cache misses).
  