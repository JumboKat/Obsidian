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
//since t is a constant pointer, we cannot change its value. We must copy it to p first, then increment p.
```
### Multi-Dimensional Array
Like with a 1-D array, the identifier of an array points to its starting address. Example:
```cpp
int t[3][4];
//t is considered an array of 3 elements, each of which is an array of four ints. Thus, t is not a type int * but rather a "pointer on blocks of 4 int" type.
```
Here, the following hold true:
- t\[0] ≡ &t\[0]\[0]
- t\[1] ≡ &t\[1]\[0] ≡ t + 1
Incrementing t moves it to the next array element (not integer element); in other words, the next 4-int block in memory.