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

It behaves as if the function returned by value (p is a separate variable and thus gets a copy), so there is no difference here. In addition, the value being returned must not be local to the function, else it is destroyed before the value can be copied. In this case, the value needs to be global or static.

The real purpose is for use in overloading certain operators; `[]`, `=`, `+=`, and `<<` all return references.
#### lvalues
An **lvalue** is something that can appear on the left side of `=`, meaning it refers to an actual storage location. A function that returns by value gives a temporary (an **rvalue**) so you cannot assign anything to it. A function that returns a reference provides the variable itself, allowing us to do this:
```cpp
int& f();
f() = 2 * n + 5; // We store the statement result to the reference provide by f

f() = x; //store the value of x to the reference provided by f; normal conversion rules still apply, so  x is converted to an int first before being stored.

//A more familiar example; the [] operator returns int&, which we are allowed to assign 10 to.
v[2] = 10; 
```
#### Returning a Constant
If a function is returning a reference, it cannot return a `const` value, as the caller is at risk of modifying something that is unmodifiable.
```cpp
int n = 3; //global variables
float x = 3.5;

int& f1(...){
	return 5; //illegal
	return n; //allowed
	return x; //illegal
}
```

