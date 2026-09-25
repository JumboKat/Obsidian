Pointer-to-pointer operations can only be performed between pointers of the same type.
### Comparing Pointers
Comparing pointers of the same type means comparing the address held by each pointer. Below is an example:
```cpp
int t[10];
int* p;
for (p = t; p < t + 10; p++) *p = 1;
//We iterate until the address pointed to by p is outside the range of the array t.
```
### Pointer Subtraction
Subtracting two pointers of the same type returns the number of elements between the two corresponding addresses. The use case of this is rare.
### Assigning Pointers
Without explicit conversions, C++ only allows the allocation of the value of a pointer to a pointer of the same type. 

The sole exception is the literal integer 0, which can be assigned to a pointer of any type since it represents pointing to nothing. A pointer that points to 'nothing' is a **null pointer**. C++ defines a named constant that pointers can be assigned to instead of writing 0:
```cpp
const int NULL = 0;
```
As a constant, NULL can never be reassigned and will always mean zero.