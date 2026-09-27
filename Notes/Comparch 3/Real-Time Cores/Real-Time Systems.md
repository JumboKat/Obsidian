A **real-time system** is a system where the effectiveness of its responses is dependent on the correctness of its result *and* the time at which it is delivered. 

For real-time systems, the design target is the **worst case**, not the average. We care about **worst-case execution time (WCET)** and worst-case response time. Often, design choices that make a CPU fast also makes its timing unpredictable. For instance, a cache's timing is dependent on whether it hits or misses, which can be unpredictable. Translating virtual memory is also unpredictable, as a miss in the TLB leads to a long page-walk. To address these issues, real-time systems make use of [[Tightly-Coupled Memory (TCM)||TCMs]] and [[Memory Protection Unit (MPU)||MPUs]], respectively
### Types of Real-Time Systems
- **Hard**: a missed deadline is failure (e.g. ABS braking, airbags)
- **Firm**: a late result is useless but does not lead to failure (e.g. a dropped video frame)
- **Soft**: lateness only degrades quality (e.g. a laggy menu)

