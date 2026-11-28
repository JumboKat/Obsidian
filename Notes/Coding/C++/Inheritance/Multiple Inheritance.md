Unlike in Java, C++ allows a class to inherit from multiple base classes. Most things that apply to [[Simple Inheritance||simple inheritance]] still apply, but there are several unique issues that multiple inheritance introduces.
### Implementing Multiple Inheritance
Consider a class `Pointcol` that inherits from two other classes `Point` and `Col`:
```cpp
class Point {
	int x, y;
	public: 
		Point(...) {...}
		~Point() {...}
		void display() {...}
};

class Col {
	short col;
	public:
		Col(...) {...}
		~Col(...){...}
		void display(){...}
};
//specify protection status for each inheritance
class Pointcol : public Point, public Col { 
	...
};
```

The constructor of the derived class must be able to transmit arguments to both base constructors. The constructor here would be `Pointcol(...) : Point(...), Col(...)`. The constructor order of call would be: 
- Base class constructors, in the order they are declared in (Point, then Col).
- Constructor of derived class (Pointcol).
The destructors would be called in reverse order to this.

When multiple member functions have the same name in different classes, the range operator is used to distinguish them:
```cpp
void display(){ // in Pointcol
	Point::display();
	Col::display();
}
```

If one of the base classes was derived from another class, it would also be possible to access one of the members (if it was a public derivation), using the range operator several times. This is also the case for data members with the same name.
### Virtual Classes
![[Pasted image 20261127192555.png]]
Consider two derived classes B and C that inherit from the same base class A, and another derived class D that inherits from both B and C. In this scenario, D inherits from A twice; once through B and once through C.


![[Pasted image 20261127192608.png]]