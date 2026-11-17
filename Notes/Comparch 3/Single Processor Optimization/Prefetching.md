The p![[Pasted image 20261117175617.png]]
A loop without prefetching has to wait for memory between every computation. 

Circuits next to L1/L2 watch the miss address. After a certain number of misses establishes a pattern, they guess the stream and prefetch the next addresses in the pattern. **Stride** prefetchers can catch fixed jumps (etc. every third line is a new matrix column). Prefetching is invisible to the software and on by default. It can be tuned through two variables: **distance** (how far ahead it fetches) and **degree** (how many lines it requests per fetch).

We can measure how well a prefetcher performs based on the following metrics:
- **Coverage**: fraction of misses it removed.
- **Accuracy**: fraction of prefetch lines actually used.
- **Timeliness**: arrived before needed, but not so early that it was evicted beforehand.