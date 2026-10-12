Vector processors are specialized to explicitly exploit [[Data-Level Parallelism (DLP)||DLP]]. They can be a side component to a main processor or the processor themselves. Vector processors read data from memory into large vector registers, operate on the entire array of elements with a single opcode, and scatter results back.
### Benefits
- **Energy efficiency**: fetch/decode/scheduling is done only once for the entire array of elements.
- **Hides memory latency**: element operations are pipelined; while one element waits, others keep flowing.
- **Explicit independence**: the instruction guarantees that elements have no dependencies, so hw does not need to perform hazard checks.