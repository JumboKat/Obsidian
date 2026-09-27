An **MPU** is a small set of registers that define a base address, size, and permission attributes. The MPU enforces access permissions on a small range of fixed physical addresses. It provides protection and checks each access; it does not translate the addresses. Latency is fixed. Compared to an MMU, which enables dealing with virtual addresses, it is fast and reliable, as the MMU can trigger a variable-latency page walk on a TLB miss.

The classic Cortex-R5F has an MPU only and does not support virtual memory, keeping timing consistent.

