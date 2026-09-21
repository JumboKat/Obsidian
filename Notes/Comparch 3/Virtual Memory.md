Software programs use virtual addresses; to a program, memory is seemingly infinite. Hardware accesses physical addresses. The OS maintains the mapping between the two by dividing virtual memory into fixed-sized **pages** (commonly 4 KiB). This comes with the following benefits:
- Isolation: processes can't accidentally touch each other's memory.
- Private flat address spaces: each program acts like it has the whole address space to itself.
- Demand paging: pages are loaded into RAM only when needed, rather than all at once.
### Page Translation
Virtual memory addresses need to be translated into physical memory addresses. 
![[Pasted image 20260921185146.png]]
- The **virtual address** is comprised of a **VPN** (*Virtual Page Number*) and a page offset (select which byte in the page to access)
- The **MMU** (*Memory Management Unit*) takes the VPN and looks up its mapping to a **PFN** (*Physical Frame Number*) in a page table. 
- Once the PFN has been obtained, it is combined with the page offset (which is a direct copy from the virtual address) to create the physical address, which is then used by the CPU to access memory.
#### Page Tables
A page table is a long list containing every VPN→PFN mapping, so it is very large and must reside in main memory. Thus, the trip to fetch this mapping, called the **page walk**, takes a long time. 
Because the size of a flat table would be huge, it is divided into multiple levels.

![[Pasted image 20260921190417.png]]
With multiple levels, the VPN is sliced into indices, each served by one level.
#### TLB: Translation Lookaside Buffer
The TLB is a specialized [[Caches|cache]] that stores recent VPN→PFN mappings so that most translations are a fast lookup instead of a long page walk. It is tiny, often fully or highly associative, and single-cycle. Like caches, it is hierarchical, with a small and fast L1 TLB backed by a larger L2 TLB. A TLB miss triggers a page walk, then fills the TLB.

In the TLB, the VPN serves as the tag, while the corresponding PFN + permissions are the data of each entry.

####