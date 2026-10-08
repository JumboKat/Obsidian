There are three main hazards with ILP:
- **Read After Write (RAW)**: when an instruction reads from a register written to in the previous instruction. This is a true hazard and is unavoidable.
- **Name dependencies (WAR, WAW)**:
	- Write after read: writing to a register that is read in the previous instruction. This means we cannot reorder instructions for more efficient execution, as this will change the result of the previous instruction.
	- Write after write: Same issue; writing after a write disallows us from reordering instructions; the order in which a register is written can influence its involvement in other operations.
	- Name dependencies can be removed by renaming the register we write to.
- **Control dependencies**: branches; mitigated by prediction and speculation.