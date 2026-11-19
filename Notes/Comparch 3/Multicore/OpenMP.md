### Processes
A **process** is a program in execution. A process has its own dedicated address space,  one or more threads and OS resources. The OS keeps a **process control block (PCB)** that stores the process's metadata: PID, state, saved registers and PC, page-table pointer, open files, and scheduling info.

Processes are isolated from each other; they share memory only by explicit request, either through shared-memory segments or message passing.
### Threads
A process' address space is shared by its many threads. 
- Shared: global/static data and the heap.
- Private per thread: registers, PC, stack (for local variables), thread-local storage.
There are three kinds of 'thread':
- **User threads**: managed by a runtime library (Java virtual threads, OpenMP tasks).
- **Kernel threads**: scheduled by the OS
- **Hardware threads**: cores or SMT contexts on which kernel threads run.
The OS schedules software threads onto [[Multithreading Support||hardware threads]]. 
#### Cost of Parallelism
![[Pasted image 20261119120335.png]]
Calling `fork()` creates a copy process. A process costs about 4$\times$ as much as a thread, as it requires a new address space, page tables, and copy-on-write. Entering an OpenMP parallel region with an existing thread pool costs about 1 $\micro$s, which is 40$\times$ cheaper than creating a thread. A parallel region is only worth if it contains much more work than this overhead.
### Fork-Join Model
![[Pasted image 20261119120557.png]]
A parallel region implements **Single program multiple data (SPMD)**. A master/initial thread runs a serial segment, then **forks** a team of threads at each parallel region. These threads run code in parallel. Those that finish early wait at the **join** until all other threads have completed, allowing the next serial region to run.

POSIX Threads is the low-level, manual way of implementing this. It is flexible but verbose and prone to mistakes. OpenMP generates this pattern from one line.

Since the order in which threads execute lines is **nondeterministic**, operations on shared variables must be made **mutually exclusive**. 
### Data-Sharing Attributes
OpenMP declares attributes for variables with different levels of privatization:
- `shared`: one copy exists for all threads, the default.
- `private`: a new, uninitialized copy is given to each thread. Each thread sets the variable before using it.
- `firstprivate`: a private copy initialized with the original value.
- `lastprivate`: value from the sequentially last iteration is copied out.
Ex:
```c
int a = 1, b = 2, c = 3;
#pragma omp parallel num_threads(4) 
	private(a) firstprivate(b)
{
  b += omp_get_thread_num();   // own copy
  #pragma omp atomic
  c += b;                      // shared
}
printf("%d %d %d\n", a, b, c); // prints 1 2 17
```
a is made private, so any changes made by the thread do not reflect on the original. `firstprivate(b)` means b starts out as 2 for all threads. Changes made to it also do not affect the original. Since c was not made private, the changes do affect the original, shown in the final print statement.
### Parallel Loops
```c
#pragma omp parallel for
for (i=0; i < 16; i++) y[i] = a*x[i] + y[i];
```
OpenMP allows multiple threads to divide up the work of a loop in parallel. The iteration space is divided among them, with the default being `schedule(static)`: contiguous blocks.
![[Pasted image 20261119123107.png|407]]
For this to work, iterations must be independent from each other.

For parallel non-looping code, tasks instead enter queues. Idle threads take them (called **work stealing**, popping the oldest/largest tasks), which balances the load. It works well for recursion, irregular data, and pipelines.
#### Reduction
![[Pasted image 20261119123305.png|346]]
**Reduction** is a method for 'reducing' the output of multiple threads down to a single result. Each thread gets private copies of the data. They work in parallel without sharing or synchronization. Then, at the end, results are combined into the original variable.
#### Placing Threads in Memory
![[Pasted image 20261119123838.png|405]]
In multicore, threads can either all share one memory (best when they share data), or spread out and use each core's memory (best for bandwidth-bound loops).
##### Example
- 2 NUMA nodes each with its own memory. Local access is 100 ns, remote is 180 ns.
- 4 threads: T0 and T1 run on node 0, T2 and T3 run on node 1, and the OS can't move them.
```c
double *a = malloc(N * sizeof *a); /* no pages yet */
double *b = malloc(N * sizeof *b);

/* BAD: only the master thread (node 0) writes,
so every page is placed in node 0’s memory */
for (long i = 0; i < N; i++) { a[i] = 0; b[i] = 1; }

/* GOOD: thread k first writes quarter k, so its
pages are placed in thread k’s own node */

#pragma omp parallel for schedule(static)
for (long i = 0; i < N; i++) { a[i] = 0; b[i] = 1; }

/* compute: same schedule, so thread k again
gets quarter k: its data is local */
#pragma omp parallel for schedule(static)
for (long i = 0; i < N; i++) a[i] += 2.0 * b[i];
```

![[Pasted image 20261119125403.png]]
Without a parallel pragma, the master thread (node 0) does all the writes. Every page of a and b land in node 0's memory (due to **first touch policy**). This is the serial init. When `schedule(static)` assigns each quarter of the iterations to the threads as contiguous chunks, T0 and T1 have local access to their data, but T2 and T3 must access it remotely since all data resides on node 0, with node 1's memory controller sitting idle. This gives an average access time of $0.5\times 100+0.5\times 180=140ns$.

With a parallel pragma, each thread initializes its own quarter of the data, meaning each of its pages are placed on its own node. This is the parallel init. The average access time is 100ns since all threads can access their data locally. A NUMA-aware layout doubles the bandwidth.
#### Looping Schedules
![[Pasted image 20261119143937.png]]
- **Static**: equal, contiguous blocks decided at compile time. Zero run-time overhead.
- **Static, c**: each thread's block is divided into chunks of c executed in round robin.
- **dynamic, c**: idle threads grab a chunk f c from a shared counter.
- **guided, c**: chunk size scales with the remaining process code.
- **auto/runtime**: let runtime or `omp_schedule` decide.
Runtime is set by the slowest thread. On heterogenous, static is a load-balancing problem. Use dynamic/guided or tasks.
### Scalability
$T1$: time of the best serial program; $T_P$: time on P cores.
- Speedup: $S(P)=T_1/T_P$
- Efficiency: $E(P)=S(P)/P$
- Cost: $C(P)=PT_P$
- Overhead: $T_o(P)=PT_P-T_1$, includes all non-useful work (serial parts, idling (imbalance), synchronization, extra memory stalls)
##### Example
Given $T_1=0.135s$, $T_P=0.070s$
- $S=0.135s/0.070s=1.93$
- $E=1.93/2=0.96$
- $T_o=2(0.070)-0.135=0.005s$
**Superlinear speedup** is when speedup exceeds the number of cores. With P cores you get P private caches. If the data doesn't fit in one core's cache but each core's share does, every core runs faster than the serial core did because it hits in cache instead of having to go to DRAM. 

![[Pasted image 20261119150859.png]]
**Strong scaling**: fixed input, more cores. Ideally, $T_P∝1/P$. Work becomes faster.
**Weak scaling**: problem grows with cores. Amount of work done in the same time grows. Ideally, $T_P$ stays constant.
#### Karp-Flatt
The **Karp-Flatt** metric flips Amdahl's on its head; instead of assuming a serial fraction and calculating speedup, it instead measures speedup and solves for the serial fraction e:
$$e=\frac{1/S(P)-1/P}{1-1/P}$$
- If $e$ is constant as $P$ grows, then a true serial fraction is the limiter; parallelize more code.
- If $e$ is increasing with $P$, overhead is growing with the core count P, which could be due to synchronization, load imbalance, saturated memory bandwidth, or coherence traffic.
- 