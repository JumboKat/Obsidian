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
**Hot code** is the small part of the program where most time is spent (e.g. an inner loop executed $10^9$ times)

The problem: for an if statement, the compiler does not know which branch is the most common. It must guess which side to make its optimizations on. 