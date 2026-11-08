### Transmission by Reference
It is ideal to transfer arguments by reference when dealing with large objects:
```cpp
Point operator+(Point& a, Point& b);
```
If these were transferred by value, this would require making a copy of both objects on every call. Passing by reference simply passes an address, which is much cheaper.

However, the return value should never be passed by reference (`Point& operator+()`). Since it is created locally in the function, it is destroyed after the function call. Passing by reference would simply be passing the address of a released memory location. In the worst case, this may appear as a silent bug, as the memory may not be wiped at the time the function ends, but simply overwritten later, which is undefined behavior.

This can be avoided by dynamically allocating the local return variable. However, this is more expensive than making a copy, and complicates the program, as the memory location must be freed at some point outside of the function. Hence, it is best to return by value.

To protect the reference arguments from being modified by the function, we can use `const` in the header: `Point operator+(const Point& a, const Point& b);`. The caveat here is that if the object contains a pointer to dynamically allocated data, const only protects the pointer and not the data it points to.
### Returning a Value by Reference
A function can return a value by reference. However, when used in an ordinary way:
```cpp
int& f(){
	return n;
}

int p;
p = f();
```

It behaves as if the function returned by value (p is a separate variable and thus gets a )