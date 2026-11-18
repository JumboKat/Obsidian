### The Single Core Limit
![[Pasted image 20261117200230.png]]
The industry moved to multicore as single core hit physical limits:
- **Dennard Scaling**: as transistors shrank, voltage could drop too, so power density stayed constant. Smaller transistors meant faster chips at the same power. This ended around 2005, where leakage current prevented lowering voltage any further.
- **Power wall**: with voltage V constant, raising frequency f raises voltage, which raises power comsumption by a power of two ($P_{dyn}=\alpha CV^2f$)
- **ILP Wall**: wider OoO cores give diminishing returns.
- **Memory wall**: DRAM latency can't keep up with processor speed
- **Moore's law** meant more transistors on a chip, so the only use for them was more cores.
#### Example
![[Pasted image 20261117195651.png]]
Given the baseline of 1 core at power 1 and throughput 1. 
- For a faster single core at 1.25f, you would need 1.25V, meaning $P=1.25^3=1.95$ for ≤ 1.25x the speed.
- With two cores at (0.8V, 0.8f):$$P=2\times 0.8^3=1.02 \\ throughput =2\times 0.8 = 1.6$$
So for the same
