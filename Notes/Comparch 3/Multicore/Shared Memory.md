### Locks and Barriers
![[Pasted image 20261118212901.png]]
A **lock** provides mutual exclusion. A processor must gain lock access before it can run a **critical section** (section of code that touches shared variables). Afterward, it releases access via **unlock** to allow another process to run the section.

A **barrier** is a time gate that enforces event ordering. No processor can continue running past it until all previous tasks have finished.

Processors do not execute their code in a determined order. If two different processors try to increment the same counter, the second one can read before the first one finishes updating the counter's value. They both read 0 and both add 1 to the counter. The expected result was count = 2, but the actual result was count = 1.
### Atomic Operations
An **atomic operation** is one that is fully run without interruption; no other processor can observe it until it is done.

**Atomic read-modify-write (RMW)** is a specific atomic operation provided by hardware that performs a read, compute, and write-back in one indivisible step. Examples include **test-and-set** (read variable value to a reg and try to write 1 to it. This only succeeds if the value was loaded as 0. The 1 serves as a lock to others), swap, fetch-and-add, and compare-and-swap. 
#### Load-linked / Store-conditional (LL/SC)
Nothing is locked with this implementation. On any load+store operation, load the value, then check if the value has been modified since it was loaded. If so, the store fails. Otherwise, it goes through.
#### Test-and-test-and-set lock (TTAS)
Unlike normal TAS, TTAS first does a regular read of the value. Only when the lock looks free does the processor try a TAS. If it loses the race for lock, it goes back to reading (spinning).