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
Interconnects
#### Topologies
![[Pasted image 20261119154229.png]]
**Direct** networks have every router attached to a node (ring, mesh, torus, hypercube). **Indirect** networks (crossbar, multistage, fat tree) contain switches without nodes. On-chip, ring and 2D mesh are the most common topologies. Between servers, torus and fat tree are the most common.
#### Topology Metrics
- **Degree** $d$: the maximum links at any one router; the router's cost in ports.
- **Diameter** $D$: the longest shortest path between any pair of nodes (the worst-case distance)
- **Bisection width**: the fewest links you must cut to split the nodes into two equal halves; how much traffic can cross the middle.
- **Links** $I$: the total number of links; the wiring cost.
- **Merit**: $I/dD$
