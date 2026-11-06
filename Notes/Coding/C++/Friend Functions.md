Normally, a member function of one class cannot access the private members of another class. When defining a class, it is possible to declare **friend functions** that belong outside the class that are allowed to access private data, just like any member function.

There are several situations for friend functions:
- Independent function, friend to a class.
- A member of one class, friend to another class.
- A friend of several classes
- All member functions of one class, friends to another class.
### Independent Function
We can introduce an independent function as a friend to a class in its statement. 
```cpp
class Point {
	int x, y;
	public:
	Point(int abs = 0; int ord = 0) { x = abs; y = ord;} //inline
	friend int coincide(Point, Point); //friend function
};

int coincide(Point p, Point q) {
	if ((p.x == q.x) && p.y==q.y) return 1;
	else return 0; 
}
```

Since coincide was declared a friend function of Point, it is able to access the private members x and y.
### Class Member Function
Similar to the independent function case, except that we specify the class the friend function belongs to using the resolution operator `::`.
```cpp
class A
{
	friend int B::f(char, A);
};

class B 
{
	int f(char, A);
};
int B::f(char ..., A ...);
```
### Friend Function to Several Classes
