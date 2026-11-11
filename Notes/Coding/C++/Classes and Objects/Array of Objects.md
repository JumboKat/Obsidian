### Object Without a Constructor
For objects without a defined constructor (or a constructor with no arguments), an array of such objects can be declared in one statement without issue:
```cpp
class Point {
	int x, y;
public:
	void init(int, int); //initializes fields after construction
	void display();	
};

Point curve[20]; //Creates 20 Point objects

for (i = 0; i < 20; i++)
	curve[i].display(); // curve[i] refers to a single Point object
```

An objects array itself is not an object (unlike Java, where an objects array is an object; even a simple array of primitives is an object). It is a C-style array pointing to several objects. We can also define an array of objects as a class member:
```cpp
class Curve {
	Point p[20];
}
```
### Object With a Constructor
If the object does not contain a constructor without arguments, the previous statement will result in an error. In this case, an **initializer list** can be used to pass values to a matching constructor for the corresponding object in the array. If the list does not contain enough values for all elements in the array, a constructor that accepts no arguments (or has default values), will be used for the remaining elements.
```cpp
class Point {
	int x, y;
	public:
	Point(int abs=0, int ord=0){
		x = abs; y = ord;
	}
};

int main() {
	int n = 3;
	Point curve[5] = {{7,1}, n, 2*n+5};
}
// This creates the following points: (7,1), (3,0), (11,0), (0,0), (0,0)
```
### Dynamic Array of Objects
The **new** operator can be used to dynamically create an array of objects: 
`Point* adcurve = new Point[20]`

This example allocates memory for 20 Point type objects, placing the address in adcurve. If the Point class does not contain any constructors with no arguments, this leads to an error (except if there is no constructor defined). It is not possible to provide an initializer.