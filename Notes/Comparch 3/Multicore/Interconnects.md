### Why no Buses?
A bus is a single medium that is sequential. Users send requests to a **Bus Arbiter**, which grants access to one at a time depending on some priority scheme. This arrangement does not scale with the number of users well. Additionally, as wire length grows it become slow and power-hungry.

There are two independent fixes:
- Replace the bus with a scalable network, which has no shared medium and allows multiple communications at a time.
- Replace broadcast with a directory that contacts only caches that are on the line.
### Networks
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
Each node has an (x,y) coordinate. Subtract the source coord. from the destination coord. to find out the direction and distance. When routing, we move in the X first, then Y. This is deterministic and avoids deadlock. **Livelock**, where packets keep moving but never arrive, is also avoided. **Virtual channels**: separate buffer queues for different directions. If one flit is deadlocked in one direction, packets going to other directions can still pass through by going to a separate queu. This also helps prevent deadlock (though alone cannot entirely avoid it). 
#### Flow Control
![[Pasted image 20261119162450.png|479]]
There are three protocols for controlling the flow of packets:
- **Store-and-forward (SAF)**: a router receives the whole packet before forwarding. $T_{SAF}=H(t_r+F)$. 
- **Virtual cut-through (VCT)**: forward a flit as soon as it arrives, buffers the whole packet when blocked.
- **Wormhole**: like VCT, but only for a few flits at a time. This makes it so that on a block, flits are spread out over several routers. 