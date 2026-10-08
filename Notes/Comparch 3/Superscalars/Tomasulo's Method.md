Designed in 1967 by R. Tomasulo from IBM, **Tomasulo's Method** was created to facilitate out-of-order instruction execution to mitigate long memory-access delays and run instructions concurrently with no help from the compiler. It runs on three core ideas:
- **Reservation stations** are sets of buffers before each functional unit for issued instruction operands to wait.
- **Busy bit + tag** on every register. The busy bit is set when an instruction writes to that register. The tag names the unit that will produce this value.
- **Common Data Bus (CDB)**: a bus that forwards results from every FU back to all reservation stations and registers. The result is forwarded to dependent waiting instructions before it is written to a register.

### Dual-Issue Example
In the example below, the following instructions are executed:
- I<sub>1</sub>: ADD R2, R3, R4
- I<sub>2</sub>: ADD R2, R2, R1
There is a WAW dependency (I2 writes to R2 after I1 does) and RAW dependency (I2 reads from R2 after I1 writes to it).
![[Pasted image 20261008185227.png]]
1.  I<sub>1</sub> is issued: operands from R3 and R4 are placed into the reservation station at address 6. Since R2 is being written to, its busy bit is set to 1 and its tag is set to 6 (indicating which instruction is writing to it).
2.  I<sub>2</sub> is issued in the same cycle (dual-issue processor). R1 is placed at address 7 in the reservation station. Since R2's busy bit is set to 1, we instead take its tag and place it at the reservation station. Since I<sub>2</sub> is writing to R2, we replace its tag with 7, indicating where the result is coming from.
3. I<sub>1</sub> executes, while I<sub>2</sub> waits for it (the adder is busy AND it depends on the result).
4. I<sub>1</sub> finishes executing; in the same cycle, its result is forwarded to the reservation station address with the tag 6. The tag tells the CDB where to forward it. 
5. In the same cycle, I<sub>2</sub> begins executing. After it is done, its result is forwarded to the register tagged with 7. The tag bit is removed.
6. In the next cycle, the result is written into R2, and with no more instructions writing to it, the busy bit is set to 0.

Though this design has since been improved upon, the three core ideas: reservation stations, the CDB, and tagging, all remain an integral part of the design.