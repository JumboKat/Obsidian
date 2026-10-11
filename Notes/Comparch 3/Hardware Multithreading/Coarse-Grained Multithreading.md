With coarse-grained multithreading, each thread runs until it blocks on a long-latency event, which triggers a switch. Each time we switch, the pipeline is flushed. While this incurs greater overhead, this means there is less thread state information since there is only one thread in the pipeline at a time. When it is flushed out, a thread simply restarts fetching from its saved PC.

![[Pasted image 20261011163738.png]]
Conceptually, this is the closest to OS multitasking, but the switch is done by hardware between resident contexts.
### How Many Threads?
![[Pasted image 20261011164822.png|344]]
Each thread runs for R cycles, then waits L cycles; a switch costs C cycles for N resident threads.
**Linear region** (not enough threads):
$$U=\frac{NR}{R+L}$$
**Saturation** (optimal thread count):
$$U_{max}=\frac{R}{R+C}$$
