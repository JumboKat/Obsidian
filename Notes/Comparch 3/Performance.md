### Performance Metrics
- **Execution Time/Latency**: the time to complete one request/task. 
- **Throughput**: amount of work completed per unit of time.
- **Speedup**: the relative runtime improvement after a change is made.
- **Parallel efficiency**: speedup divided by resources (# nodes).
- **Peak compute**: maximum arithmetic rate.
- **Memory bandwidth**: bytes moved per second.
- **Power**: rate of energy use.
- **Energy/op**: energy per useful operation.
#### Performance Equation
$$T_{CPU} = IC \times CPI \times T_{clk} = \frac{IC \times CPI}{f_{clk}}
$$
Where:
- $T_{CPU}$ = Execution time
- IC = Dynamic instruction count
- CPI = average cycles per instruction
- $T_{clk}$ = clock period
This equation shows that performance is heavily determined by computer architecture (instruction count and CPI) not just clock frequency.
#### Little's Law
For any stable system over a long interval:
$$L = λW$$
or 
$$λ=\frac{L}{W}$$
Where:
- L = average number of jobs in the system, including jobs waiting and currently being serviced.
- λ = average rate at which jobs are completed (throughput)
- W = average time a job spends in the system (wait time + service time)
Little's law explains that throughput is equal to concurrency over latency.
#### Amdahl's Law
Proposed at a 1967 conference by Gene Amdahl at IBM.