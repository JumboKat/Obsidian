### Why no Buses?
A bus is a single medium that is sequential. Users send requests to a **Bus Arbiter**, which grants access to one at a time depending on some priority scheme. This arrangement does not scale with the number of users well. Additionally, as wire length grows it become slow and power-hungry.

There are two independent fixes:
- Replace the bus with a scalable network, which has no shared medium and allows multiple communications at a time.
- Replace broadcast with a directory that contacts only caches that are on the line.
### Network-on-Chip (NoC)
![[Pasted image 20261119154058.png]]
- A **node** on the network represents a core/cache tile, memory controller, and I/O connected to a router via a network interface. 
- A **link/channel** is the wires between two routers, with width $w$ bits. It facilitates one transfer per cycle.
- A **packet**: a message split into **flits** (flow-control units), with a head at the front.
- A **hop** is one router-to-router step. 
With interconnects, the LLC is split into **slices**; accesses to data on remote slices must travel through the network, incurring a hop cycle.
#### Topologies
![[Pasted image 20261119154229.png]]
**Direct** networks have every router attached to a node (ring, mesh, torus, hypercube). **Indirect** networks (crossbar, multistage, fat tree) contain switches without nodes. On-chip, ring and 2D mesh are the most common topologies. Between servers, torus and fat tree are the most common.
![[Pasted image 20261119161443.png]]
- Crossbar: any input connects to any output. No contention for links, but requires $N^2$ endpoints. Used inside routers and for small core counts.
- Multistage: $log_2N$ stages of N/2 small switches; cheaper, but some permutations block. Crossbar is still more common.
#### Topology Metrics
- **Degree** $d$: the maximum links at any one router; the router's cost in ports.
- **Diameter** $D$: the longest shortest path between any pair of nodes (the worst-case distance)
- **Bisection width**: the fewest links you must cut to split the nodes into two equal halves; how much traffic can cross the middle.
- **Links** $I$: the total number of links; the wiring cost.
- **Merit**: $I/dD$
#### X-Y Routing
![[Pasted image 20261119161854.png]]
Each node has an (x,y) coordinate. Subtract the source coord. from the destination coord. to find out the direction and distance. When routing, we move in the X first, then Y. This is deterministic and avoids deadlock. **Livelock**, where packets keep moving but never arrive, is also avoided. **Virtual channels (VC)**: separate buffer queues for different directions. If one flit is deadlocked in one direction, packets going to other directions can still pass through by going to a separate queue. This also helps prevent deadlock (though alone cannot entirely avoid it). 
#### Flow Control
![[Pasted image 20261119162450.png|479]]
There are three protocols for controlling the flow of packets:
- **Store-and-forward (SAF)**: a router receives the whole packet before forwarding. $T_{SAF}=H(t_r+F)$. 
- **Virtual cut-through (VCT)**: forward a flit as soon as it arrives, buffers the whole packet when blocked.
- **Wormhole**: like VCT, but only for a few flits at a time. This makes it so that on a block, flits are spread out over several routers. $T_{CT}=H(t_r+1)+(F-1)=Ht_r+F$
	- Time = # hops(router time + 1) + (# flits - 1) = $Ht_r+F$ 
	- router time + 1 = head flit, # flits -1 = rest of flits
- Wormhole is popular as it minimizes buffer size.
#### Internal Router Structure
![[Pasted image 20261119174807.png|478]]
The diagram above shows the internal structure of a router in a NoC. There exist five ports in a mesh: N, E, S, W, and L (Local, the tile attached to this router). Each port has 2 VC queues, with 10 in total. A $5\times 5$ crossbar connects any input to any output. There are six pipeline stages the head flit goes through:
- **Buffer Write (BW):** the flit arrives and is written to a VC buffer.
- **Route Compute (RC)**: read the destination and pick the output port.
- **VC Allocation (VA)**: Reserve a free VC in the next router's input.
- **Switch Allocation (SA)**: Each cycle, arbitrate which VC gets to use each output port, and set the crossbar.
- **Switch Traversal (ST)**: The flit crosses the crossbar.
- **Link Traversal (LT)**: The flit travels along the link to the next router.
The rest of the body flits inherit the same route, skipping the RC and VA steps. Routing decisions are made once per packet, not per flit. With **credit-based flow control**, each router keeps a count of the free slots in the downstream router's VC. SA only lets a flit go if a credit is available. Real NoCs cut this down to 1-2 cycles per hop by using **lookahead routing** (compute the route for the next router in advance so that RC is off the critical path) and **speculation** (VA and SA are done in parallel, assuming VA will succeed).
### Why No Ordered Broadcast?
Snooping requires a broadcast and a total order. A mesh gives neither for free; a request costs N messages per miss, and requests reach different nodes in different orders. With the physical address space being partitioned over the nodes, an address tells you which memory holds the line (its home), not which node caches hold it. This is what the [[Directories||directory]] does.