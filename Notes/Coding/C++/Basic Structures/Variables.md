Declaring a variable uses the following syntax: type v; (ex. int p; double x;).
- Variable definition can appear anywhere in the program, but a variable must be declared first before it is used (C++ processes code sequentially).
- The scope of a variable includes the end of the first composite statement (marked by }).
- Variables that are defined outside of any function and any namespace is a **global variable**.
### Initializing Variables
We can initialize variables when we declare them in one of two ways:
- type v = val; (e.g. int p=25; double x=12.34;)
- type v(val); (e.g. int p(25); double x(12.34);)

If a variable is set but not initialized, C++ does not assign it a default value; it only reserves a chunk of memory, and the variable is initialized to whatever value was sitting there. In essence, the value can be anything; it can differ each time the program is run.
### Constant Variables
A constant variable cannot have its value changed after it is initialized. The syntax for declaring a constant variable is: 
```cpp
const type name = val;
//example
const int Length = 10;
```
### Strings
Strings are not a primitive data type in C++, but rather a class. To use them, you must include its header file at the top of the code:
```cpp
#include <string>
```

We can declare a string variable via its constructor and pass up to two arguments. The following are all valid declarations:
```cpp
string t;
string word = "hello";
string ch2 (10, '*');
```
When using the constructor, the first argument sets the size of the string, while the second initializes each character of the string.
- s.size() returns the length of a string s.
- s\[i] returns the i-th character of the string s.
- s+t returns a new string that is a concatenation of strings s and t.
