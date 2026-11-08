C++ allows overloading of operators with respect to an object. For example, if a and b belong to a class, we can overload the following operators:
- a + b
- a - b
- a * b
- a/b

An operator can only be over-defined if if there is at least one class-type argument. It is impossible to over-define an operator on basic types, with the exception of the `new` and `delete` operators which can be changed globally (for all objects and basic types).

Java does not allow operator over-definition.
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
