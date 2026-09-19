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