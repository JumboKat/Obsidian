![[Pasted image 20261010160858.png|416]]
A core with multithreading support includes:
- On-core storage for several contexts. Since thread states are nearby, switching takes only a few cycles.
- With switches being so cheap, the OS is actually too slow to schedule threads; the core decides which thread to fetch from.
- Granularity is a design choice:
![[Pasted image 20261010161108.png]]
- [[Coarse-Grained Multithreading]]: switch when a thread encounters a long-latency event.
- [[Fine-Grained Multithreading]]: switch threads every cycle.
- [[Simultaneous Multithreading (SMT)]]: run instructions from multiple threads in the same cycle.

**Vertical waste** (empty rows) occurs when no instruction runs; this is addressed by coarse- and fine-grained MT (as well as SMT). **Horizontal waste** (partially-empty rows) is addressed by SMT.