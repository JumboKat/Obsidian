In contrast to [[Multicore Processors||multicore]] and [[Multithreading Support||multithreading]] processors that exploit thread-level parallelism, many workloads instead apply the same operation to many independent data items - **data-level parallelism.**
Examples:
- Image/video/audio codecs
- Convolution
- Cryptography
- Databases
- String search

We can reduce the cost of fetching, decoding, renaming, and scheduling over N operations by executing one instruction on N elements.
### Flynn's Taxonomy
![[Pasted image 20261012124710.png]]
Here, we are interested in **SIMD** (Single Instruction, Multiple Data)