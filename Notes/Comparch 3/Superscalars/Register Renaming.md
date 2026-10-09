Architectural registers (a register the programmer/compiler can name, belonging to the ISA) are few (only a few bits dedicated for the register name). Physical registers are plenty. Architectural names can create false [[Dependencies||dependencies]] (WAW and WAR). Thus, each architectural register is mapped to a physical one. Physical registers that are unmapped exist in a list of free registers. Every write destination is assigned a new physical register from the free list.

- The **RAT** performs the arch→phys mapping. 
- The **free list**: a list of free physical registers. Refilled at commit.
- **Dependence-Check Logic**: 

