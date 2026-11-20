![[Pasted image 20261119183025.png|519]]
The **directory** is a table attached to the cache that records, for each cache line, which caches currently hold a copy and in what state. It replaces the bus-based broadcasts. An address tells you where the **home** of a line is (the place in memory that holds it), but not which node caches have copies. The directory lives at the home and stores exactly that. Every miss goes to the home, which:
1. Orders requests for that line
2. Looks up who has copies.
3. Contacts only those caches.
### Directory Entry
![[Pasted image 20261119184138.png]]Each line in the cache has an associated directory entry that stores:
- **State**: uncached, shared, or modified. Caches still run MSI/MESI, but without bus transactions.
- **Presence bits** (for full-map directories): one bit per cache. A 1 in a cache's column means it holds a clean copy. In modified state, only the owner's bit is set.
### Transactions
The following are agents involved in a transaction:
- **L**ocal: the cache that missed and is requesting a line.
- **H**ome: the node whos memory hold the line. It keeps the line's entry and orders the requests for it.
- **R**emote: the owner of a modified copy.
- **S**harers: nodes with clean copies.
A node can play several roles (e.g. the requester is also the home).
#### MSI with a Directory
![[Pasted image 20261119184627.png]]
MSI still uses the same states and events as before. Now, bus transactions are messages to the home, and snooped events are messages from the home. There is also no listening; a cache in S is not told about other readers; a cache in I hears nothing.
#### Read Miss
A read miss can lead to 1 of 3 scenarios:
![[Pasted image 20261119184924.png]]
- a) clean at home: local sends a request to home, who sends back the data since it holds it.
- b) L sends a request to H. The directory says the line is dirty, so H sends a fetch request to the holder R. R sends the data back to H, who then sends the data back to L
- b) Same as b), but R now forwards the data to L directly (while also sending a copy to H).
#### Write Miss
![[Pasted image 20261119190914.png]]On a write miss, L sends read-exclusive to H. H sends invalidations to every sharer (in parallel), it sends the data back to L along with the number of acks to expect. Sharers then invalidate the line and send an ack to L. L writes once it has the data and all the expected acks. The directory now says M for that line with L as the owner.

On any eviction, the node goes silently to I, but sends a replacement hint to H so that its presence bit in the directory can be updated.
### Directory Size
There are two types of directories: 
- **Classic full map**: N presence bits for every memory line. Overhead does not scale well (size grows with memory lines x nodes) as the number of cores N grows. If memory grows in proportion to N, directory size grows $O(N^2)$.
	- Ex: a full map of a 512 GiB DRAM has 8 Gi lines$\times$ 66 bits = 66 GiB, most of which is unused.
- **Sparse directory**: a directory that only keeps entries for lines that are currently cached somewhere (privately; L1/L2) rather than every single line of DRAM. This is what is actually used in modern chips. Lines that aren't held in cache don't have an entry (costs nothing), while entries that do exist now also need a tag indicating the line (since not every line has an entry). Size = entries $\times$ bits per entry (tag, state, sharers). A cache can fill up, however. When a new line needs an entry and the set is full, and existing entry must be evicted, which requires invalidating all the copies that entry tracks. Thus, we over-provision entries (1.5x).
	- Ex: 64 cores $\times$ 2 MiB L2 = 128 MiB private cache total. 128 MiB/64 B per line = 2 Mi lines of cache. $2\ Mi\times 1.5=3\ Mi$ entries.
### Limited-Pointer Directories
A **limited-pointer directory** shrinks each directory by storing a few pointers to sharers, rather than one presence bit per core. Most lines only have a handful of sharers at a time anyway. A limited-pointer directory, written as Dir<sub>i</sub>, stores only i pointers each entry of $log_2N$ bits for N cores.
- Ex: i=4, N=1024 cores, then:
	- each pointer is $log_21024=10$ bits.
	- 4 pointers = 40 bits per line vs 1024 bits for a full map.
	- 40 / 512 bits per 64 B line = 7.8% overhead vs. 200% for full map.
Pointers win on size when $ilog_2N << N$
### Directory Slices
![[Pasted image 20261119200043.png]]
The classic directory sits with DRAM with an entry for every line in memory. This is too big, and too far from the cores. Modern chips put a sparse directory slice next to each cache that holds only entries for lines that are cached, each with a tag. 
### Finding Home
 Example: 18 cores split among two die; tiles 0-8 on die 0 and 9-17 on die 1. 40-bit addresses. SNC is on, meaning each die is its own NUMA node. die 0's memory controllers hold 0-64 GiB, while die 1's hold 64-128 GiB, each memory is homed to that die's tiles (if SNC was off, homes would be hashed over all 18 tiles). Each L3 slice is 2 MiB, 16-way, 64 B lines means 2048 sets.
 - sets = slice size / (ways x line size) = 2 MiB / (16 x 64 B) = $2^{11}=2048$.

The address being looked up: X = 0x10_1234_57A4
![[Pasted image 20261119225537.png|572]]
Step 1: Strip the offset, since we are need the line, not the specific word.
A line is 64 B, which can be addressed by the lowest 6 bits; this is the offset, which is 0x24. This leaves 0x4048D15E.

Step 2: Which die?
$2^{36}$ bytes = 64 GiB, so bit 36 says which half of memory the address is in. Bit 36 = 1, meaning it is in die 1's memory. This is where the home is (with SNC on).

Step 3: Which tile on die 1?
Die 1 has 9 tiles. This example uses line address mod 9. 0x4048D15E mod 9 = 4, so home is tile 9+ 4 = 13.

Step 4: lookup inside CHA 13. The L3 slice in tile 13 is an ordinary cache, so the address is split again. 6 bits make up the offset, $2048=2^{11}$ sets means the index is 11 bits, and 40-11-6=23 bits for the tag (0x8091A). This is compared with the 16 ways to find the line.
### Protocol Deadlock
Two requests for the same line can be in flight at once. The home keeps the entry busy until the current transaction finishes; requests coming in later are NACKed (retry) or queued. The protocol must not assume FIFO order since messages can arrive at different speeds.

**Protocol deadlock**: a request waits for a response stuck behind requests in the same buffers. The fix: separate virtual networks per message class.
![[Pasted image 20261120000048.png]]