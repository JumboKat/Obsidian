![[Pasted image 20261117143522.png|379]]
A [[SIMD in General-Purpose CPUs||SIMD]] register is a wide register split into equal lanes, with one instruction operating on all at 
once. Scalar code only uses the first lane, leaving the rest idle.
### Auto-Vectorization
The compiler vectorizes a loop of scalar instructions (runs each independently in a separate lane) at `-O3` only if it knows that running them simultaneously gives the same result as running them one by one; there must be no dependencies between the loops.
```c
//this code can vectorize; c[i] does not need c[i-1]
for (i = 0; i < n; i++)
	c[i] = a[i] + b[i];

//this code cannot; a[i] is dependent on a[i-1]
for (i = 1; i < n; i++)
	a[i] = a[i-1] * 0.5f;
	
//maybe not: aliasing
void f(float *x, float *y, int n){
	for (i = 0; i < n; i++)
		y[i] = 2 * x[i];
}
```
In the last example, the issue is **aliasing**: if the caller passes `y = x+1`, then writing `y[i]` overwrites `x[i+1]`. The compiler can't see the caller, so it must assume the worst case and keep it scalar.

The fix for aliasing: declaring the pointers with the `restrict` promises the compiler that the pointers will never overlap. However, if the promise is broken, the behavior is undefined, which may lead to a silent bug.