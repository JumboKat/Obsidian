Simple inheritance is a core concept of O.O.P, allowing for the definition of a **derived** class inheriting all the members of a **base** class, while adding its own new ones. Unlike Java, C++ also allows [[Multiple Inheritance||multiple inheritance]], where a class is derived from several base classes.

Take the class Point:
```cpp
class Point {
	int x, y;
	public:
		void initialize(int abs, int ord) { x = abs; y = ord; }
		void move(int dx, int dy) { x = x + dx; y = y + dy; }
		void display() {cout << "I am in " << x << " " << y << "\n";}
};
```

We can define a class called `Pointcol` that inherits the features of the `Point` class, while adding a variable called `col` for the colour and member function `colour` to assign a colour to the coloured point. So, `Pointcol` is a *Point-derived class*. We declare a derived class with the form `class derived_class : public base_class`:
```cpp
class Pointcol : public Point {
	short col;
	public:
		void colour(short c) { col = c; }
};
```
The `public` keyword means that the public members in the base class (`Point`) will also be public members of the derived class (`Pointcol`). Hence, each `Pointcol`-type object can call public methods of `Pointcol` and public methods of `Point` as if they were a member of `Point`.