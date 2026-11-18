![[Pasted image 20261117192134.png]]
The table above shows energy expenditures for different operations. DRAM access costs by far the most; moving data costs more, not computing it. Instruction overhead dwarfs the instructions themselves. This is why SIMD saves so much energy, as one overhead pays for several operations.

Locality and blocking save energy in proportion to the energy they remove; vectorization saves pre-instruction overhead.