A general-purpose CPU core can perform a limited form of vector processing via **SIMD**. For some applications, like dealing with multimedia data, data can be short (8-bit/16-bit). On hardware designed for 64-bit operations, this wastes most of the space, while still costing one fetch/decode for each of these smaller values. Instead, we partition the existing hardware to accommodate multiple smaller values.
### Partitioning
![[Pasted image 20261012142639.png]]
For example, given a 64-bit adder, it can be partitioned to serve eight 8-bit values at once. We simply block the carries to format the size (e.g. block the carry at every 8 bits/1 byte for 8-bit values, or every second byte for 16-bits).

This works for loops, where the same operation is performed i times, and **straight-line code (SLP)**, where we have independent segments of identical shape (ex: R+=XR c1, G+=XG c2, B+=XB c3 all become one 3-element operation).
### Packed Registers
