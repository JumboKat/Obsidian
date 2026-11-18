![[Pasted image 20261117192134.png]]
The table above shows energy expenditures for different operations. DRAM access costs by far the most; moving data costs more, not computing it. Instruction overhead dwarfs the instructions themselves. This is why SIMD saves so much energy, as one overhead pays for several operations.

Locality and blocking save energy in proportion to the energy they remove; vectorization saves pre-instruction overhead.
### DVFS vs Race-to-Idle
For a fixed task, is it cheaper in energy to run slowly (DVFS) or run fast and then sleep (race-to-idle)?
Power has a static and a dynamic part:$$P=P_{static}+P_{dyn} \\ P_{dyn}=\alpha CV^2f$$
- $P_{static}$ is constant leakage, paid as long as the system is awake.
- $P_{dyn}$ is the power from actually switching transistors. It scales with voltage squared $\times$ frequency.
- **DVFS (dynamic voltage and frequency scaling)**: lowers $f$ and $V$ together. Power falls fast, but the task takes longer.
- **Race-to-idle**: runs at full speed, finish early, then go into a deep sleep state (a **C-state**) where power is near-zero.
Since energy = power $\times$ time, the winner depends on $P_{static}$
#### Example:
A task takes 1s to compute at full speed ($V_0,f_0$), with $P_{dyn}=4 W$
**DVFS**: half the frequency and 80% of the voltage, so $f=0.5f_0, V=0.8V_0$:
$$\frac{P_{dyn}}{P_{dyn,0}}=0.8^2\times 0.5=0.32$$
