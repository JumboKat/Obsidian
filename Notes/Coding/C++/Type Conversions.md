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
A constructor that can be called with a single argument doubles as a conversion function. Take the following example:
```cpp
void draw(Point p) {...}

draw(3);
Point b = 5;
```
If the compiler sees a type mismatch, it calls the single-argument constructor to perform a "conversion" by creating a new object of the desired type initialized by the original type, as long as the constructor accepts that type. Without such a constructor, an error occurs. We can do this between two user-defined classes as well (e.g.`Complex(Point)` converts Point into a Complex). A constructor cannot convert an object of its class to a simple type (Point to an int). For cases that cannot be solved by the constructor, we define an appropriate **cast operator**.
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
##### Implicit Cast During a Function Call
