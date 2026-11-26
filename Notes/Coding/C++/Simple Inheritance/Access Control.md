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
- Protecte
