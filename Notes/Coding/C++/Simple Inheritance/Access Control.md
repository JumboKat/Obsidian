There are three privacy derivations with respect to inheritance: **public**, **private**, and **protected**. With public, the derived class B has access to all public members of the base class A, as well as anyone using a B-type object, with private members of A remaining inaccessible.

The **protected** status serves as a middle ground between public and private. Protected members in the base class are public to all derived classes but private to the outside world, including users of a B-type object. 
```cpp
class Point {
	protected:
		int x, y; // are now usable by derived classes
	public:
		Point(...);
		display();
		...
};

class Pointcol : public Point {
	short col;
	public:
		void display() {
			cout << x << y << col;
		}
};
```

Since friend functions have total access to members of the derived class, they will also have access to protected members in the base class. However, declarations of friendship are not inherited; if A declared a friend function f, it is not automatically a friend of derived class B.
### Public Derivation
The derivation `class Pointcol : public Point{ ... }` mean:
- Public members of the base class are accessible to member and friend functions of the derived class, as well as users of the derived class.
- Protected members of the base class are accessible to member and friend functions of the derived class, but not to users of the derived class.
- Private members of the base class are completely inaccessible to the derived class. 
- All members of the base class retain their status in the derived class.
### Private Derivation
Using `private` instead of `public` prohibits users of the derived class from accessing the base class' public members, but the derived class itself still has access. Essentially, public base class members behave like private members of the derived class:
```cpp
class Point {
	...
	public:
		Point(...)
		void display();
		void move(...)
};

class Pointcol : private Point {
	...
	public:
		Pointcol(...);
		void colour(...);
};
```

The following calls are rejected by the compiler:
```cpp
p.display() // or p.Point::display()
p.move() // or p.Point::move()
```
While `p.colour()` is still accepted.

This has two specific use cases:
1. All the useful functions in the base class are redefined in the derived class, so there is no reason to access the old ones.
2. Adapt an interface: the derived class adds nothing new and behaves like the base class, but with presents the user with a different set of functions.
### Protected Derivation
Deriving using the `protected` keyword makes all public members of the base class behave like protected members in all derived classes.