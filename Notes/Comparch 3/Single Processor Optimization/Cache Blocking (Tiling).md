### The Problem
Given the following NxN matrix multiply of C = A x B:
```c
for (i = 0; i < N; i++)
  for (k = 0; k < N; k++) {
    double a = A[i][k];
    for (j = 0; j < N; j++)
      C[i][j] += a * B[k][j];
  }
```
The multiply performs $2N^3$ operations (three loops of an add and a multiply) on $3N^2$ numbers. Every element of B is used N times; lots of reuse. 

![[Pasted image 20261117172153.png]]
The problem is that with N = 1024, B is 8 MB but L1 is only 32 KB. By the time the next iteration of i (i+1) needs $B[0][.]$ again, it has already been long evicted. So every use of B is a miss and requires a trip from a slower level. The loop has been designed for reuse, but the reuse distance is larger than the cache.
### The Standard Solution
Split the matrices into $T\times T$ tiles, then multiply by one tile at a time. The outer loops iterate through the tiles, while the inner loops iterate up until the length of each tile.
```c
for (ii = 0; ii < N; ii += T)
  for (kk = 0; kk < N; kk += T)
    for (jj = 0; jj < N; jj += T)
      for (i = ii; i < ii+T; i++)
        for (k = kk; k < kk+T; k++) {
          double a = A[i][k];
          for (j = jj; j < jj+T; j++)
            C[i][j] += a * B[k][j];
        }
```
First, choose a tile size $T$ so that three tiles can fit into the L1 cache:$$3T^2\times 8B≤32,768\ B \\ T ≤ 36 \\ T = 32$$ The cache size is 32 KB, and we use 24 KB with three tiles.

Then, count loads from the next level. There are $(N/T)^3$ tile multiplications, each tile multiplication loads the B and C tiles from L2, which is $2T^2$ words. The A tile stays since the jj loop is the innermost of the tile loops, hence the same A tile is reused across it (B and C depend on it, A does not, and every iteration of jj is a new tile).

This brings the total number of words from $N^3$ down to $(N/T)3\times 2T^2=2N^3/T$ words. IN this case, T/2 = 16x fewer loads.

The remaining 8 KB of the cache is deliberately left as slack. We can't use the entire cache due to set-associativity. 