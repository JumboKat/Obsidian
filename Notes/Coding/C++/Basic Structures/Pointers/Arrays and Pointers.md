The name of an array is actually a constant pointer to its first element.
### One-dimensional Arrays
For an array declared as: 
```cpp
int t[10];
```
The following statements are true:
- "t" is then equivalent to &t\[0] (The address of the first element).
- t+1 is equivalent to &t\[1].
- t+i is equivalent to &t\[i].
- t\[i] is equivalent to \*(t+1).
#### Populating the Array
We can populate a 1-D array in one of two ways:
```cpp
int i;
for (i = 0; i < 10; i++)
	*(t+i) = 1;
//increment the pointer each step and set the value at its address to 1
```
OR
```cpp
int i;
int *p;
for (p = t, i = 0; i < 10; i++, p++)
	*p = 1;
```