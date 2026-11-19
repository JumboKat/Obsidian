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