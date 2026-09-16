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

