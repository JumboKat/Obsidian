Every object of a derived class B contains a base-class (A) object within it called the **base subobject**. Creating any instance of B first creates the A part first, then building the rest of B on top of it. This means an automatic call to A's constructor, followed by B's constructor that completes whatever is specific to B. 

Similarly, when a B-type object is destroyed, an automatic call is made to B's destructor that handles what is unique to B, followed by A' destructor.
### Transmitting Information Between Constructors
A problem arises when the base constructor requires arguments. In this case, the derived class' constructor can specify which arguments it wants to transmit to the base constructor. For the class Point and its derived class Pointcol:
```cpp
class Point {
	...
	public:
		Point (int, int);
		...
};

class Pointcol : public Point {
	...
	public:
		Pointcol(int, int, char);
	...
}
```

We specify that the first two arguments are to be passed to the base constructor with:
```cpp
Pointcol(int abs, int ord, char c) : Point(abs, ord)
```
Thus, the statement `Pointcol a(10, 15, 3);` leads to the following:
- A call to the `Point` constructor with the arguments 10 and 15.
- A call to the `Pointcol` constructor with the arguments 10, 15, and 3.
The call `Pointcol q(5,2)` will be rejected if there isn't a matching constructor signature in `Pointcol`. However, we can remedy this with default arguments:
```c++
Pointcol (int abs = 0, int ord = 0, char c = 1) : Point(abs, ord)
```
Thereby making the call `Pointcol b(5)` possible. The presence of default arguments in Point has no impact here.






