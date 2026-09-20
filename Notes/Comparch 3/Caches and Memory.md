#ComparchIII
To achieve peak performance, a CPU needs fast access to memory for fetching instructions and r/w data. Historically, improvements in processor performance have eclipsed those of DRAM latency. This is known as the **memory wall**, where the speed of the processor is bottlenecked by the speed at which memory can deliver data. 

A **cache** seeks to bridge the speed gap. It is a small but fast memory close to the core that stores copies of recently used data so that most accesses don't need to fetch from main memory. This arrangement tricks the CPU into thinking it has access to both big and fast memory.
### Locality of Reference
Caches take advantage of **locality of reference**, which can be broken down into two types:
- **Temporal Locality**: the tendency of an address that was recently used to be accessed again.
- **Spatial Locality**: the tendency that if one address is used, its neighbors are likely to be accessed as well (e.g. arrays, instruction streams).
This means that only a small working set of data is active at any moment. Keeping this working set in a fast cache means that most accesses are hits. 

Memory accesses are often made in **blocks**, which includes a desired address as well as the words that follow it (enough to fill one line in the cache). This means that even a miss brings several neighbors, which can mean more hits in the future.
#### Example
![[Pasted image 20260919165852.png]]
In this example, two loops compute the same result but interact with memory very different. The first accesses data with a stride of 1, while the second accesses data with a stride of N. In the first program, since we are accessing each neighbor, each miss is followed by 15 consecutive hits. With a stride of N, each memory access lands on a new line, and each line in the cache is evicted before it is accessed again, resulting in close to one miss per memory access. 

So while we are performing the same operations with the same result, the stride-1 version is several times faster because it respects spatial locality.
### Accessing the Cache
Knowing where to access desired data within a cache requires a way of mapping blocks to locations in memory. The data's address can be used as an identifier when split into three fields:
![[Pasted image 20260919171132.png]]
- **Offset** tells us which word in the line we are accessing.
- **Index** tells us which line we are accessing.
- **Tag**: as main memory is much larger than cache, multiple data must map to the same address in the cache. The tag allows us to identify which address in memory a specific address in the cache corresponds to via comparisons.
##### Example
![[Pasted image 20260919172323.png]]
This example shows how to calculate the offset, index, and tag in a direct-mapped and 4-way set associative cache.
###### Direct-mapped
Each block (or line) holds 64 bytes, so the offset is the number of bits that can represent 64 positions (log<sub>2</sub>64 = 6 bits). So we can use a 6-bit offset to address 64 words.

The index is used to address the different lines of the cache. The total size of the cache is 32KiB (1 kibibyte is 1024, so this is 32x1024 = 32768). To get the number of lines, we divide the total size of the cache by the size of each line:
$$\frac{32768}{64}=512$$
Then, the number of bits that can address 512 lines is:
$$log_2512 = 9$$
So, the index is 9-bits. The tag is simply the remaining bits in the address. Since each address is 32 bits, this means that the tag is 32-6-9 = 17 bits.
###### 4-way Set-Associative
The address for this is similar, except that since this is a 4-way set-associative cache, each index can point to four different blocks. With a cache of the same block size and total size, the # of sets is a quarter of what it was originally (128 sets), so the index is 7 bits. This leaves two more bits for the tag, which is now 19 bits.

The **key takeaway**: increasing the number of ways shrinks the index but grows the tag; as more lines share the index, we need more bits in the tag to distinguish between them.
#### Direct-mapped Cache
A direct-mapped cache boasts the simplest design; every block in memory has one and only one line it can occupy in the cache, chosen by its index. It is simple and inexpensive to build, and since there is only one tag comparison, it is fast and low power. However, if several blocks with a high access rate share the same index, conflict misses can occur.

A direct-mapped cache is a set-associative cache with one way.
![[Pasted image 20260919175754.png]]
The following occurs when accessing a block in cache:
- Index selects the line to access.
- The tags are compared.
- If tags match and the data is valid, the access is a hit.
- The offset is used to select the desired byte from the block.

![[Pasted image 20260919180042.png]]
Because each address in main memory maps to only one line in the cache, conflict misses can be significant if two or more hot blocks contest for the same lines, even when the rest of the cache is empty.
#### Set-associative Cache
An N-way set-associative cache gives each block N possible homes (one per way). A **set** is a group of lines that share the same index. This means the cache is much more flexible than a direct-mapped cache, with fewer conflict misses, while able to maintain near-equal access time. However, increasing the number of ways means increasing the number of tag comparisons, making implementation costlier and accesses slower and energy-intensive.

![[Pasted image 20260919180500.png]]
The following occurs when accessing a block in cache:
1. The index chooses the set to access.
2. The address' tag is compared to the tag of all blocks in the set simultaneously via comparators. Either zero ways can match (miss; block isn't in cache) or one can match.
3. The offset mux uses the offset to choose which word in the selected block to access.
#### Fully-associative Cache
In this cache design, a block can go anywhere in the cache. It is an all-ways set-associative set with a single set. While this provides the maximum flexibility and minimizes conflict misses, this design requires *every* tag in the cache to be compared on every access; search the cache by what it contains rather than where it is (called **content associative memory or CAM**). This means that accesses are expensive, power-hungry and slow, and so all small structures (TLBs, small L1s) are fully-associative. 

Here indexes are not used; the address is comprised of only a tag and offset.
### Replacement Policies
A miss occurring into a full set means we must evict one resident line (the **victim**) to bring the new data into cache (called a **capacity miss**). In direct-mapped cache, there is only one option. Associative caches use a replacement policy:
- **Round-robin/FIFO**: cycle through the ways; cheap but not optimized for reused data.
- **Random**: pick any way. Trivial hardware and robust for large, high-associativity caches.
- **Least-recently used (LRU)**: evict the line unused for the longest. Respects temporal locality, but true LRU requires ordering hardware, so pseudo-LRU approximations are used.
### Write Policies
#### Write Misses
The **allocation policy** determines whether a write that misses pulls the block into the cache.
- **No-write-allocate**: A write miss goes straight to memory and does not bring it to cache. 
- **Write-allocate**: A write miss fetches the block from memory and writes to the cache.
#### Write Hits
A policy also determines what to do on a write hit:
- **Write-through**: update the cache and memory together and keep them in sync; simple but increases write traffic.
- **Write-back**: update only the cache and set a dirty bit; write to memory later (on eviction, flush, or coherence event). Less traffic, but the data in memory is temporarily stale. This is most popular in modern multi-level caches.
### Multi-level Caches
A cache can't be both fast and large, so we use a hierarchy of caches instead, with smaller, faster caches close to the core and larger, slower caches near memory. A miss at a higher level (closer to core) is served by the next level down.
![[Pasted image 20260919182754.png]]
L1 is the smallest and fastest and located on the core. It is usually split into instruction and data caches so that fetch and load/store units get a dedicated port.
#### Shared Caches
![[Pasted image 20260919183304.png]]
In multi-core systems, caches can be private or shared. L1s are almost always private, while lower level cores can be shared. Once a level is shared, every level closer to memory is also shared.
- Shared caches use space flexibly and speed up core-to-core communication (data does not need a round trip to DRAM)
- Private caches guarantee capacity for their respective core and are faster to access.
- Sharing is where coherence is important.
#### Inclusion Policy