C++ allows the conversion of both simple types and user-defined class types.
### Simple Type Conversions
Conversions can be either **implicit** or **explicit**. Implicit conversions are performed by the compiler, while explicit conversion is done by the programmer using a cast operator.
#### Explicit Conversion
```c++
int n; double z;

//converting int to double

z = double(n); 
//or
z = static_cast<double>(int);

// converting the other way

n = int(z);
//or
n = static_cast<int>(z);
```
#### Implicit Conversion
Implicit conversion is carried out by the compiler in the following scenarios:
- **In assignment**: forced conversion into the type of the receiving variable.
- **In function calls**: forced conversion of an argument into the declared parameter type.
- **In expressions**: possible conversion of one operand into the type of the other following specific rules:
	- systematic conversions: char and short to int.
	- type adjustment conversions: int to long for the addition of two long types.
### Conversion of Class Types
#### Cast Operator
We define an explicit cast operator (of form type(object)) for each type we want to convert an object into. These should always be defined as a public (to be usable outside of the class) member function, with no return value type declared. Consider the class Point:
```cpp
class Point {
	int x, y;
	public:
		Point(int abs = 0, int ord = 0){
			x = abs; y = ord;
		}
		operator int() {//convert Point into an int equal to its x value.
			return x;
		}
};

int main() {
	Point a(3,4), b(5, 7);
	int n1, n2;
	
	//explicit call of int()
	n1 = int(a); //can also write (int)a or static_cast<int>(a)
	
	//implicit call of int()
	n2 = b;
	cout << n1; << " " << n2 << endl; // prints 3 and 5
}
```
##### Implicit Casts
The appropriate cast operator is implicitly called when an object is passed to a function expecting a different type.

Take the example:
```cpp
void fct(int n){...}

Point a(3, 4);
fct(a); //compiler calls int(a)
```
The conversion is done well before the function is called, and there is no copying creation of a Point object.

An implicit call is also made during an evaluation of an expression:
```cpp
Point a(3, 4), b(5, 7);
int n1, n2;
n1 = a + 3; // n1=6
n2 = a + b; // n2=8

double z1, z2, z3;
z1 = a + 3; // z1 = 6
z2 = a + b; // z2 = 8
z3 = a + 3.85; // z3 = 6.85
```
For the expression `a+3`, the compiler first looks for an overdefined +operator corresponding to the two operand types. When it doesn't find one, it then tries to set up conversions of operands to lead to a valid operation. In this case, it converts a Point to an int to perform integer addition.

With z1 and z2, a and b are converted to int type, then integer addition is performed, the result of which is converted to a double.

In the case of z3, the conversions are chained: Point objects are converted to ints first, then from ints to doubles.
##### Ambiguity in Chain Conversions
Once a chain of conversions has been set up by the compiler, ambiguities may occur. If the Point class was defined with the two member functions `operator int()` and `operator double()`, then the expression `a + 3.85` (a being a Point object), leads to two possible conversion schemes:
![[Pasted image 20261124180923.png|447]]
In this case, an error occurs, and the expression is rejected altogether due to the ambiguity of having two chains of conversions allowing the move from Point to double.
#### Conversion Constructor
A constructor that can be called with a single argument doubles as a conversion function. If the compiler sees a type mismatch, it calls the single-argument constructor to perform a "conversion" by creating a new object of the desired type initialized by the original type, as long as the constructor accepts that type. Without such a constructor, an error occurs. We can do this between two user-defined classes as well (e.g.`Complex(Point)` converts Point into a Complex). 

Take the following example:
```cpp
class Point {
	int x, y;
	Public:
		Point(int abs = 0, int ord = 0){
			x = abs; y = ord;
		}
}

void fct(Point p) {...}

int main(){
	void fct(Point); 
	Point a(3, 4);
	a = Point(12); // explicit call to constructor
	a = 12; // implicit call
	fct(4); // implicit call
	return 0;
}
```
The call `a = Point(12)` creates a temporary Point-type object initialized with 12 and assigns it to a. The call `a = 12` does the same thing via an implicit call. The call `fct(4)` creates a temporary Point initialized with 4 and passes it to fct.

If the function's prototype was `fct(Point &)`, then `fct(4)` would be rejected. Declaring it as `fct(const Point &)` however would accept it, first creating a temporary Point object with 4 and then transmitting its reference to the fct function.

A constructor cannot convert an object of its class to a simple type (Point to an int). For cases that cannot be solved by the constructor, we define an appropriate **cast operator**.
##### Chain of Conversions
A constructor can also be implicitly called in a chain of conversions. Take the Complex class:
```cpp
class Complex {
	double real, imag;
	public:
		Complex(double r = 0, double i = 0);
};
```
If the call `Complex c = 3` is made, the 3 is first converted from int to double, then from double to Complex via the constructor.