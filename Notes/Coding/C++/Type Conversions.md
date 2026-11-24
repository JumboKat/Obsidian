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
A constructor that can be called with a single argument doubles as a conversion function.