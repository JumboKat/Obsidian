### Locks and Barriers
![[Pasted image 20261118212901.png]]
A **lock** provides mutual exclusion. A processor must gain lock access before it can run a **critical section** (section of code that touches shared variables). Afterward, it releases access via **unlock** to allow another process to run the section.

A **barrier** is a time gate that enforces event ordering. No processor can continue running past it until all previous tasks have finished.

Processors do not execute their code in a determined order. If two different processors try to increment the same counter, the second one can read before the first one finishes updating the counter's value. They both read 0 and both add 1 to the counter. The expected result was count = 2, but the actual result was count = 1