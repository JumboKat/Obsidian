A function is defined using the following syntax: type name(list of parameters) {body}, where:
- type is the type of the data returned (use **void** if the function returns nothing).
- The body is the list of instructions to be carried out. It uses its own local variables, global variables, and formal parameters.
- If the function returns a result, there must be a **return expr** statement.
- If nothing is returned, we can still use **return;**
### Function Calls
To call a function, we use the form: name (list of arguments)
- Each formal argument isa an expr that must be compatible with the type of the respective formal parameter.
#### Passing Parameters by Value
By default, parameters are initialized as a copy of the values passed to it (in Java, this only applies to primitives). This means that changing the value of a parameter within the function does not change the value of the actual variable passed to it.
Example:
```cpp
void swap(int a, int b)
{
	int c;
	c = a;
	a = b;
	b = c;
	cout << "swapping ends: " << a << " " << b << endl;
}
#include <iostream>
using namespace std;
int main(){
	void swap (int a, int b);
	int n=10, p=20;
	cout << "before call: " << n << " " << p << endl;
	swap(n,p);
	cout << "after call: " << n << " " << p << endl;
	//The output after call would be 10 20 since original values were unaltered.
}
```
#### Passing Parameters by Reference
If we want changes within a function to affect the original variables we pass it, we must pass them by reference using **&**:
```cpp
#include <iostream>
using namespace std;
int main(){
	void swap (int &, int &); //we can omit the names of arguments; only put the type 
	int n=10, p=20;
	cout << "before call: " << n << " " << p << endl;
	swap(n,p);
	cout << "after call: " << n << " " << p << endl;
	//The output after call would be 20 10
void swap(int &a, int &b)
{
	int c;
	c = a;
	a = b;
	b = c;
	cout << "swapping ends: " << a << " " << b << endl;
}
```
When passing by reference, rather than copying the values of the variables passed to a function, the parameters actually point to the same space in memory as the original variables. 
#### Default Arguments
Unlike Java, C++ allows for default values of arguments to be set in cases where the arguments are not provided when the function is called. If default arguments are set, they must be the *last* arguments defined in the function's namespace to avoid confusion on which parameters correspond to which default arguments.
```cpp
#include <iostream>
using namespace std;
int main(){
	int n=10, p=20;
	void fct(int, int=12);
	fct(n,p);
	fct(n);
	//fct() would cause a compilation error
}

void fct(int a, int b){
	cout << ... ;
}
```
Default arguments are set in the function's statement, not its definition.
### Overloaded Functions
In C++, functions can be overloaded, meaning two or more can have the same name but accept different parameters. The choice of which function is called depends on the type of arguments that are passed to it. If arguments are not an exact match, the argument(s) will be implicitly converted to the data type of the parameter of a function closes to the value passed to it:
```cpp
//example 1
void image(int);
void image(double);
char c;
float y;
...

image(c); //calls first image, converts char c to an int
image(y); //calls second image, converts float y to a double
```
#### Ambiguity
In the case where two or more functions are an equal fit and the choice is ambiguous, the compiler throws an error; there must only be one unique option for any function call:
```cpp
void test(int, double);
void test(double, int);
int n,p;
double z;
char c;

test(n,z); //call first test
test(c,z); //call second test
test(n,p); //compilation error; can either convert p into double without changing n and call first test, or convert n into double without changing p and call second test

test(); //this also throws an error due to ambiguity 
```
#### Const Parameters
When passing parameters by value, the original variable is not altered. Thus, there is no difference between the two function headers:
```cpp
void test(int);
void test(const int); 
```
In both cases, a constant int is being passed to the function. Thus, any call will invoke an error due to ambiguity.

A distinction *can* be made when passing parameters by reference:
```cpp
void test(int &);
void test(const int &);
```
These two functions are distinct; the first accepts a reference to a variable as a parameter, and any change that occurs within the function will affect the original variable. The second strictly specifies a constant int, meaning that a *copy* of the variable value is made, and this copy is treated as a constant int.

Passing by reference does not allow implicit conversion, but passing a constant reference does (we simply convert the constant copy of the value).