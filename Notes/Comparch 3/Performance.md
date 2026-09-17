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
### Performance Laws
#### Little's Law
For any stable system over a long interval:
$$L = λW$$
or 
$$λ=\frac{L}{W}$$
Where:
- L = average number of jobs in the system, including jobs waiting and currently being serviced.
- λ = average rate at which jobs are completed (throughput)
- W = average time a job spends in the system (wait time + service time)
**Little's Law** explains that throughput is equal to concurrency over latency.
#### Amdahl's Law
Proposed at a 1967 conference by Gene Amdahl at IBM, **Amdahl's Law** states that some portion of every program must be executed sequentially (β), while the rest can be executed using parallelism (1 − β). Depending on the computer architecture, the latter portion can be sped up. The law proposes a formula that calculates the speedup of the overall program given a speedup of part of the program:
$$Speedup=\frac{1}{β+\frac{1-β}{S}}$$
Where:
- β is the portion that cannot be sped up (must run sequentially)
- 1 − β is the portion that can be sped up
- S is the speedup factor
The key takeaway is that the sequential portion becomes the bottleneck: as S → ∞, T (time) →β. In addition, DMA setup, memory copies, synchronization between host and accelerator, as well as software overheads cannot be accelerated and are lumped into β.
##### Example:
Suppose 80% of runtime is matrix multiplication. An FPGA or GPU speeds up that kernel by 20x. 
- 1−β = 0.8, so β = 0.2
- S = 20
If we plug in for total speedup: $$S_{total}=\frac{1}{0.2+\frac{1-0.8}{20}}=4.17$$
This means that with a 20x speedup to the kernel, the total program speedup only amounts to 4.17x due to the 20% of the program that cannot be accelerated.
#### Gustafson's Law
Where Amdahl's law assumes the problem size stays constant when adding processors, with the serial portion serving as the bottleneck, **Gustafson's Law** instead states that in practice, adding more processors does not run the same problem faster, but rather it runs a bigger problem in the same amount of time.

The formula is derived by normalizing the runtime of P processors to 1 unit of time:$$T_P=α+(1-α)=1$$
- α is the serial portion ran by P processors
- 1-α is the parallel portion.
The parallel portion 1-α represents P units of work happening simultaneously. So, if we abstract the multiprocessor system to just a single processor doing work, it would be:
$$T_1=α+P(1-α)$$
Thus, the Law calculates how much longer the work would take on 1 processor vs P processors:
$$S_G(P)=\frac{T_1}{T_P}=\frac{α+P(1-α)}{1}=P-α(P-1)$$
##### Example:
Given P = 16, α = 0.05:
$$S_G(16)=16-0.05(15)=15.25$$
So within the same 1-hour timeframe, 16 processors can perform 15.25x more total work than 1 processor could, but this does not mean that they could perform the same small job 15.25x faster.
##### Example:
![[Pasted image 20260915161130.png]]
This table shows how processor runtimes decrease as the number of processing cores increases. It measures the speedup and calculates the parallel efficiency:$$E_P=\frac{S_P}{P}$$
- The program speeds up, but not linearly; the speedup slows as the number of cores increases
- Efficiency declines as cores increases, indicating that the serial portion serves as a bottleneck; eventually, the runtime will hit a floor, as no amount of additional processors can speed up the serial portion of the program.
### Benchmarks
Benchmarks are are defined, standardized way to quantify the behavior of a computing system, allowing us to compare systems with each other on certain performance aspects using the same benchmark. A benchmark consists of the following components:
- **Workload**: the computation the system will be tasked to perform.
- **Input**: the inputs that the system works with that define the problem instance.
- **Implementation**: exposes software/hardware interactions.
- **Measurement**: defines what is quantified (e.g. time, throughput, energy, power).
- **Protocol**: makes results reproducible. (e.g. defines how many repetitions, level of concurrency, etc.)
#### Why are Benchmarks Important?
Benchmarks provide experimental information on which architectural decision are made.
- Comparison between systems/components under common conditions (e.g. processors, GPUs, FPGAs, memories, networks)
- Design feedback: identify limiting factors of the architecture (computation, memory, software overhead, etc.)
- Reproducibility
- Procurement and deployment: select a system for a workload rather than relying on peak specifications
- Optimization

Benchmarks don't measure performance outright; they measure aspects of performance under certain conditions and parameters.
#### Comparing Systems
