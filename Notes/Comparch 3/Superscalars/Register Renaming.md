Architectural registers (a register the programmer/compiler can name, belonging to the ISA) are few (only a few bits dedicated for the register name). Physical registers are plenty. Architectural names can create false [[Dependencies||dependencies]] (WAW and WAR). Thus, each architectural register is mapped to a physical one. Physical registers that are unmapped exist in a list of free registers. Every write destination is assigned a new physical register from the free list.

- The **RAT (register allocation table)** performs the arch→phys mapping. 
- The **free list**: a list of free physical registers. Refilled at commit.
- **Dependence-Check Logic**: since multiple instructions are renamed together by the RAT, if one instruction reads from what an earlier instruction writes to, it will get the old mapping and read a stale value. The DCL overrides the RAT's output to prevent this.
### Example
![[Pasted image 20261009172201.png]]
There are three architectural instructions. The registers have initial mappings in the RAT, and there are some physical addresses that are in the free list. There is a WAW dependency between 1 and 3.

![[Pasted image 20261009172301.png]]
Instruction 1: X2 is remapped to P17 from the free list. The old register value at P34 is still available for instructions that need it (allows for out-of-order execution).

![[Pasted image 20261009173039.png]]
Instruction 2: We now read from P17 to get the value of X2 (true dependency). We remap X3 to P22 since we are writing to it.

Instruction 3: We remap X2 to P2 since we are writing to it, breaking the WAW.