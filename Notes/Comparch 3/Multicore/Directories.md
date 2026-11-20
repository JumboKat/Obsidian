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
 
![[Pasted image 20261119225537.png]]