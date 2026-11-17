### The Software Toolchain
Source code goes through a pipeline from C to a running program:
![[Pasted image 20261117120226.png]]
- The **preprocessor** expands `#include` and `#define` so the compiler sees one big text file.
- First, the **compiler** translates and optimizes the code into assembly for the target CPU.
- Then, the **assembler** turns each line of assembly into bytes.
- The **linker** joins everything, decides final addresses, connects calls to functions in other files.
- The **loader** copies the exe into memory, loads shared libraries, and starts it.
### Optimization Levels
The same C source can become very different machine code depending on the **optimization level**.
- At O0: no optimization, each statement is translated literally; used for debugging only.
- At O1/O2: **register allocation** (keep variables in CPU registers), removes redundant work, reorders instructions, **inline** (copy the body of a small function into its caller to remove call overhead) small functions.
- At O3: more inlining, loop **unrolling** (several iterations per loop trip for less branch overhead), **auto-vectorization** (compiler turns a scalar loop into SIMD instructions)
### Profile-Guided Optimization (PGO)
**Hot code** is the small part of the program where most time is spent (e.g. an inner loop executed $10^9$ times). **Cold code** rarely gets executed (error handling, start-up). 90% of the time is typically spent in 10% of the code. 

The problem: for an if statement, the compiler does not know which branch is the most common. It must guess which side to make its optimizations on. A **profile** is a measure of how often each branch was taken and each function was called, recorded on a run on typical input. During a recorded run, the compiler adds a counter increment on every branch and function entry. These are software counters in the hardware's memory, different from the CPU's performance counters in hardware. With the profile, the compiler puts the likely side of each branch on the straight-line path, inlines the functions that are called millions of times, and move cold code out of the way.

![[Pasted image 20261117142718.png|352]]
A **basic block** is a block of code only entered from the top and left from the bottom. A function can be represented as a graph of basic blocks. Hot basic blocks are run millions of times. A profile allows the compiler to distinguish blocks from each other.
### Code Layout
![[Pasted image 20261117143026.png]]
Instruction reordering places hot block together allowing them to be fetched together. Fitting them within the same cache line means fewer trips i-cache and i-TLB misses.