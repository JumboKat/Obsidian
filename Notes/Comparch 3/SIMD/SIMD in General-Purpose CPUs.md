A general-purpose CPU core can perform a limited form of vector processing via **SIMD**. For some applications, like dealing with multimedia data, data can be short (8-bit/16-bit). On hardware designed for 64-bit operations, this wastes most of the space, while still costing one fetch/decode for each of these smaller values. Instead, we partition the existing hardware to accommodate multiple smaller values.
### Partitioning
![[Pasted image 20261012142639.png]]
For example, given a 64-bit adder, it can be partitioned to serve eight 8-bit values at once. We simply block the carries to format the size (e.g. block the carry at every 8 bits/1 byte for 8-bit values, or every second byte for 16-bits).

This works for loops, where the same operation is performed i times, and **straight-line code (SLP)**, where we have independent segments of identical shape (ex: R+=XR c1, G+=XG c2, B+=XB c3 all become one 3-element operation).
### Packed Registers
![[Pasted image 20261012153720.png|320]]
A **packed register** holds several adjacent same-type elements. A **packed instruction** applies one operation to each element. The **vector length (VL)** is the number of elements per instruction
($VL=\frac{register\ width}{element\ width}$). Lanes (L) are the number of working slices in the same cycle. VL is what the instruction asks for, while L is what the hardware provides. 

In the example above, *FADD V2.4S, V0.4S, V1.4S* is a packed instruction, with V2, V0, and V1 being the registers and .4S referring to a VL of 4 elements.

To calculate the number of cycles for one instruction:
$$T=T_{start}+\left|\frac{VL}{L}\right|-1$$
- if L = VL, it is completed in one pass/cycle
- If L < VL, it takes several passes.
### Packing and Unpacking
