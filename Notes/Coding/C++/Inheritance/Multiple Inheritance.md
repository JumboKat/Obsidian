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
```