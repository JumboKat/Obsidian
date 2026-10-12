Vector processors are specialized to explicitly exploit [[Data-Level Parallelism (DLP)||DLP]]. They can be an extension to a main processor or the processor themselves. Vector processors read data from memory into large vector registers, operate on the entire array of elements with a single opcode, and scatter results back.
### Benefits
- **Energy efficiency**: fetch/decode/scheduling is done only once for the entire array of elements.
- **Hides memory latency**: element operations are pipelined; while one element waits, others keep flowing.
- **Explicit independence**: the instruction guarantees that elements have no dependencies, so hw does not need to perform hazard checks.

![[Pasted image 20261012133601.png]]
Vector operations usually take many cycles in pipelined lanes. Vector registers provide the independent elements, scalar registers provide addresses, strides, and scalar operands. The FUs are fully pipelined; a new element starts every cycle. The vector load/store unit streams elements from memory.
### Instruction Execution
The execution time of one vector instruction is $$T≈T_{start}+\frac{n}{L}-1cycles$$
where:
- n = # of elements
- L = # of lanes
- T<sub>start</sub> = pipeline depth (# of stages).
#### One Pipelined Lane
A vector instruction does not need to process the whole vector at once. The simplest solution is a single pipelined FU, with one new element entering every clock cycle. One the pipeline is full, one element per cycle is processed.
#### Multiple Lanes
![[Pasted image 20261012135512.png]]
Multiple lanes can process multiple elements per cycle. This is done by partitioning the vector register file into **lanes**, each assigned to one FU. Elements are interleaved across lanes (element i lives in lane i mod L). The lanes don't require communication for element-wise operations.
### Predication
![[Pasted image 20261012140526.png|463]]
Sometimes, only some elements take part in a computation . **Predicate registers** are a bit mask that hold one bit (or more) per element. If the predicate bit is set, we make the computation for the element. If not, we do not compute a new result. 
Example:
- e.g. if (C\[i] > 0), then A\[i] = B\[i] + C\[i].
- A vector compare runs C > 0 across all elements and writes a 1 for each that meets the criteria.
- The predicate mask becomes 1010, meaning C\[0] and C\[2] are positive, the rest are not.
#### Inactive Lanes
For an inactive lane, there are two choices as to what to do:
- **Merging**: keep the old value. Use if we are conditionally updating an existing array.
- **Zeroing**: write as zero. Use when we don't care about the old value or want zeros for clean math.

Predication means we do not need to perform a branch for each element. Every element goes down the same instruction path, and the condition is the data that gates the writes. The tradeoff is that inactive lanes still occupy hardware slots.

Lastly, predication handles elements at the tail end of a loop. If there are 70 elements with a vector length of 16, there are 4 full iterations and 6 leftover elements. The predicate cans imply set the last 6 bits to 1 and 10 empty slots to zero.
### Strip-Mining
Vector processor hardware can only process a fixed **maximum vector length (MVL)** per instruction. For large arrays of elements greater than the MVL, we divide them into **strips** of length no greater than the MVL, then process one strip per iteration.
### Chaining
![[Pasted image 20261012141948.png]]
For instructions that are dependent on the result of another, we would normally have to wait for the full result of the previous instruction to calculate. However, because elements are independent, we do not have to wait for the entire vector to calculate; rather, we can start the calculation of each individual element as soon as previous respective result is ready.
 - Example: Once element 0 of vmul is calculated, it is forwarded to vadd and it can start calculating its own element 0.