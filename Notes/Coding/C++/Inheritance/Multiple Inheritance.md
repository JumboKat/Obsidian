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
Consider two derived classes B and C that inherit from the same base class A, and another derived class D that inherits from both B and C. In this scenario, D inherits from A twice; once through B and once through C. The diagram above is equivalent to the one below:
![[Pasted image 20261127192608.png]]

This means that every member of A is inherited twice, leaving D with duplicates of the exact same members (this only really applies to data members; duplicating a function has no effect). If we want D to have two datasets from A, we can distinguish them using the range resolution operator, distinguishing:
- `A::B::x` from `A::C::x`
- or `B::x` from `C::x` if B and C themselves do not have a member x.
However, we generally do not want this duplication of data. To tell the compiler that we only want to incorporate A's members only once in all of its descendants, we use the `virtual` keyword in A's *direct* inheritors; in other words, in B and C's declarations, not D's:
```cpp
class B: public virtual A {...};
class C: virtual public A {...};
class D: public B, public C {...};
```

The keyword has no effect on the B and C classes themselves. It can be placed before or after the protection status word.
#### Constructor and Destructor Calls
If the `virtual` keyword is not used, and D had declared its inheritance from B and C in this order:
![[Pasted image 20261127193941.png]]
Then the constructors will be called in the following order:
- A1, B, A2, C, D
For information transfers, B and C may not have the same arguments with respect to A. For example:
```cpp
B (int n, int p, double z): A(n, p)
C (int q, float x) : A(q)
```
In this case, it does not matter, since virtually two copies of A are created. 

However, if `virtual` is used, then only one type A object will be created, which creates ambiguity in which constructor will pass its arguments to A. The compiler resolves this by forcing D's constructor to pass the arguments to A's constructor:
```cpp
D(int n, int p, double z) : B(n, p, z), A(n, p){}
```
D names A directly in its initializer list and supplies the arguments. Any `A(...)` initializers in B or C are ignored when creating a D object (they can still be written and are still used when creating B and C standalone objects). The virtual base A is constructed first, before B and C, then D.

Since B and C can be created as standalone objects, if either one of them has a constructor with no arguments passed to A, then A must have a constructor with no arguments to allow the creation of a standalone B or C.

In the example below, the constructor call order is F, E, G, H I (or F, E, H, G, I depending on whether G or H is declared first in I). This is because the virtual class' constructor is always called first.
![[Pasted image 20261127194939.png]]

Take the inheritance tree below as another example. `Pointcol` and `Pointmass` inherit `Point` virtually to avoid duplication in `Pointcolmass`.
![[Pasted image 20261127195114.png]]
The base classes:
```cpp
class Point {
	int x, y;
	public:
		Point(int abs, int ord){x = abs; y = ord;}
		Point(){x = 0; y = 0;} // default constr. necessary for virtual deriv.
};

class Col {
	short colour;
	public:
		Col(short c){colour = c;}
};

class Mass {
	int mass;
	public:
		Mass(int m){mass = m;}
};

// Derived classes

class Pointcol : public virtual Point, public Col {
	public:
		Pointcol(int abs, int ord, short cl) : Col(cl){} // no info for Point because virtual derivation
};

class Pointmass : public virtual Point, public Mass {
	public:
		Pointmass(int abs, int ord, int m) : Mass(m){} //no info for Point
}
```
