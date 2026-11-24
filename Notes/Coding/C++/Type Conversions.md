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
