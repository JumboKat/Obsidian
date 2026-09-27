A **safety-critical system** is one whose failure can cause loss of life, injury, environmental harm, or major damage. 
### Enforcing Safety
There are four tenets to enforcing safety; most real-time systems use all four:
1. **Fault avoidance**: this components is rigorous, and can include requirements traceability, reviews, verification, and coding standards like MISRA C. It stops bugs from being built into the system.
2. **Fault detection**: this component checks for random HW faults. This can include **Error Correcting Code (ECC)** parity checks, **Cyclic Redundancy Checks (CRC)**, lock-step comparisons, watchdog timers, and clock/voltage monitors.
3. **Fault tolerance**: this component ensures that damage is controlled and mitigated in the event of a failure. This includes implementing redundancies and fall back safe states.
4. **Timing guarantees**: this component ensures timing is consistent and predictable. This includes WCET analysis, using an RTOS with bounded scheduling latency, and deadline monitoring.
### Safety Measures
There are three main measures of performance to look for when analyzing safety:
- **FIT**: the failure rate; 1 FIT = 1 failure per 10<sup>9</sup> device-hours.
- **Diagnostic Coverage (DC)**; the fraction of dangerous faults that HW detects. 
- **Fault-Tolerant Time Interval (FTTI)**: the acceptable time a fault can persist before it causes harm (the maximum response time). Detection + response time must be less than FTTI.
### Redundancy
![[Pasted image 20260927162108.png]]Real-time SoCs have multiple cores, but they all perform the same task in lock-step. This is to ensure redundancy in case of failure on any individual core. There are two levels of redundancy:
- **Dual Modular Redundancy (DMR)**: Two cores run the same work. A comparator checks them. It can detect a disagreement, but not correct it (since there is only two, either have an equal chance of being correct); it must transition to a safe state. Lock-step CPUs are DMR.
- **Triple Modular Redundancy (TMR)**: Involves three processing units and a majority voter. In the event of a single fault, the majority voter selects the result with the most votes, so it is able to detect and correct the output; it continues to operate in the event of failure.
On the MPSoC, the A53 and R5F clusters cooperate, performing different tasks, while the cores within each cluster perform the same computation for the purpose of redundancy.
