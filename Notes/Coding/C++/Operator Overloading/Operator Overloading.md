C++ allows overloading of operators with respect to an object. For example, if a and b belong to a class, we can overload the following operators:
- a + b
- a - b
- a * b
- a/b
### Over-definition of Operators
Suppose we want to define the operator **+** to give meaning to an expression a + b, where a and be are Point type. We would have to define a function named `operator+`. This function takes two Point type arguments and returns a value of the same type. The function can be a member of the class or an independent function (typically a friend function).

In general, when multiple operators are used in the same expression, they retain their relative priority and association, whether they are over-defined or not. All operators can be over-defined, with the exception of: `.`, `::`, and `? :` (ternary operator).
#### With a Friendly Function
```cpp
class Point {
	int x, y;
	public:
		Point(int abs=0, int ord=0) {x=abs; y=ord;}
		friend Point operator+(Point, Point);
};

Point operator+ (Point a, Point b)
{
	Point p;
	p.x = a.x + b.x; p.y = a.y + b.y;
	return p;
}

//We can use + in either way:
c = a + b;
c = operator+ (a, b);
```

This example defines the + operator for the point class using an independent friend function. Being a friend function allows it to access the private members of the objects.

The expression `a + b + c` is evaluated using operator priority and association rules. So, it is interpreted as `(a + b) + c`, and is interpreted as: `operator+(operator+(a,b),c)`.
#### With a Member Function
```cpp
class Point{
	int x, y;
	public:
	Point(int abs=0, int ord=0) {x=abs; y=ord;}
	Point operator+ (Point);
};
Point Point::operator+(Point a){
	Point p;
	p.x = x + a.x; p.y = y + a.y;
	return p;
}
```

The assignment `c = a + b` is interpreted as: `c = a.operator+(b)`. However, the interpretation of the assignment `c = a + b + c` isn't explicitly specified. Some compilers create a temporary object:
```cpp
t = a.operator+(b);
c = t.operator+(c);
```

While others will do the following:
```cpp
c = (a.operator+(b)).operator+(c);
```
### Transmission by Reference
It is ideal to transfer arguments by reference when dealing with large objects:
```cpp
Point operator+(Point& a, Point& b);
```
If these were transferred by value, this would require making a copy of both objects on every call. Passing by reference simply passes an address, which is much cheaper.

However, the return value should never be passed by reference (`Point& operator+()`). Since it is created locally in the function, it is destroyed after the function call. Passing by reference would simply be passing the address of a released memory location. In the worst case, this may appear as a silent bug, as the memory may not be wiped at the time the function ends, but simply overwritten later, which is undefined behavior.

This can be avoided by dynamically allocating the local return variable. However, this is more expensive than making a copy, and complicates the program, as the memory location must be freed at some point outside of the function. Hence, it is best to return by value.

To protect the reference arguments from being modified by the function, we can use `const` in the header: `Point operator+(const Point& a, const Point& b);`. The caveat here is that if the object contains a pointer to dynamically allocated data, const only protects the pointer and not the data it points to.