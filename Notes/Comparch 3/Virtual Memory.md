Software programs use virtual addresses; to a program, memory is seemingly infinite. Hardware accesses physical addresses. The OS maintains the mapping between the two by dividing virtual memory into fixed-sized **pages** (commonly 4 KiB). This comes with the following benefits:
- Isolation: processes can't accidentally touch each other's memory.
- Private flat address spaces: each program acts like it has the whole address space to itself.
- Demand paging: pages are loaded into RAM only when needed, rather than all at once.

Virtual memory addresses need to be translated into physical memory addresses. 
![[Pasted image 20260921185146.png]]