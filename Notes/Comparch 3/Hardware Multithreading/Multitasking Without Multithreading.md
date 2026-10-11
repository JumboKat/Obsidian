![[Pasted image 20261010160549.png]]
A processor without multithreading support can multitask. The OS lets each thread run for a time slice, giving the illusion of concurrency; only a single thread runs at any given moment. 

To switch between threads, the OS must first save the current thread's registers/PC, then load the next thread's state, which can take hundreds of clock cycles and incurs a high overhead cost, which can be made even worse by cold caches, TLB misses, and branch mispredictions.

We want to be able to switch within a few cycles instead of hundreds, improve throughput, while not paying the full cost of a second core.