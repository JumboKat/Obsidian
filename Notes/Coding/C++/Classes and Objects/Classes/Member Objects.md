Like any other data, an object can be a member of a different class. If the member object contains private fields, they cannot be accessed by the object that contains it. Furthermore, if the member object has a defined constructor, the class is required to also define a constructor, since the member object must be supplied arguments. The arguments of the member object's constructor must be supplied by the argument of the class's constructor:
```cpp
class Point {
	int x, y;
	public:
	Point(int, int);
};

class Circle {
	Point centre;
	int radius;
	public:
	Circle(int, int, int);
};

Circle::Circle(int abs, int ord, int ray) : center(abs,ord){
	radius = ray;
}
```

If Point has a constructor with no argument, the Circle constructor does not need to specify arguments for its constructor.
### Multiple Objects
For classes with several member objects, the selection of arguments for different constructors is made by separating each list by a comma:
```cpp
class A {
	A(int);
	...
};

Class B {
	B(double, int);
};

class C {
	A a1;
	B b;
	A a2;
	
	C(int n, int p, double x, int q, int r) : a1(p), b(x, q),
		a2(r);
};
```

The class constructor (C) is executed at the end, after the member constructors have been executed.
### Copying
When [[Copy Constructor||copying]] an object with member objects, that object will be copied by calling its own copy constructor. 