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
### The Solution
