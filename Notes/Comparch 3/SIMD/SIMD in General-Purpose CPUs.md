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
### SIMD Overheads
#### Packing and Unpacking
A SIMD load takes consecutive memory words. If one of the values needed isn't adjacent to the others, they must first be rearranged, which takes extra instructions. How often this occurs depends on the data layout and type of data used by instructions:

![[Pasted image 20261012160353.png]]
An **Array of Structures (AoS)** has the elements belonging to each structure adjacent. An individual structure has its elements adjacent and can be accessed as a whole.

![[Pasted image 20261012160438.png]]
A **Structure of Arrays (SoA)** has each type of element organized into arrays separate arrays; elements of the same type are adjacent and can be accessed easily.
#### Alignment
Unaligned vectors can span two different cache lines, causing two separate accesses. To fix this, we can use aligned allocation or **peeling**.

SIMD works on the assumption that data is already in order. Layout is often the real bottleneck.
#### Control Flow
Lanes cannot branch individually; all lanes compute the same thing. One lane can't take the then path while another skips it. **If-conversion** computes the then-result in all lanes, then use a mask (comparator) to select which lanes take the path and which don't (use old value). This adds a cost, as we must compute both branches in all lanes, plus the compare and blend logic. 
#### Loop Tails
Problems occur when an array has a number of elements that can't be fully covered by the number of lanes (e.g. 10 elements but 4 lanes, leaves 2 left over after 2 passes). 

Computing the full vector is not possible; the last two elements do not belong to this array. On a load, the program can fault if it crosses into an unmapped page. On a store, it will overwrite the data that comes next and corrupt variables.

Partial operations are also not possible. SIMD always works on all lanes. There are no per-lane masks.
Solutions:
1. **Scalar epilogue**: the default compiler solution. We run the vector loop for as many full vectors that fit, then a scalar loop on the remaining elements (one at a time). This is simple and always safe, but this runs slowly and adds code size. 
2. **Pad the arrays**: pad the arrays so that their length is a multiple of VL. The extra elements hold junk and are ignored. Fast but wastes memory, and requires allocation control.
3. **Overlap the last vector**: shift the starting element of the last vector back so that it overlaps with the previous vector. Some elements are computed twice, but the result is the same. Only works if recomputing is harmless (idempotent).
4. **Masked load/store**: the cleanest fix, give masks to each lane.
