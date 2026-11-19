### Processes
A **process** is a program in execution. A process has its own dedicated address space,  one or more threads and OS resources. The OS keeps a **process control block (PCB)** that stores the process's metadata: PID, state, saved registers and PC, page-table pointer, open files, and scheduling info.

Processes are isolated from each other; they share memory only by explicit request, either through shared-memory segments or message passing.
