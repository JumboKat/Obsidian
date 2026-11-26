There are three privacy derivations with respect to inheritance: **public**, **private**, and **protected**. With public, the derived class B has access to all public members of the base class A, as well as anyone using a B-type object, with private members of A remaining inaccessible.

The **protected** status serves as a middle ground between public and private. Protected members in the base class are public to all derived classes but private to the outside world, including users of a B-type object. 
```cpp
class Point {
	protected:
		int x, y;
	
}
```