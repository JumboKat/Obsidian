Given the following matrix multiply of C = A x B:
```c
for (i = 0; i < N; i++)
  for (k = 0; k < N; k++) {
    double a = A[i][k];
    for (j = 0; j < N; j++)
      C[i][j] += a * B[k][j];
  }
```