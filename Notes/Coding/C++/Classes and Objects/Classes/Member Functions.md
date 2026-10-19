### Overloading
All member functions, including [[Constructor and Destructor||constructors]] can be [[Functions#Overloaded Functions||overloaded]]. The status (private/public) of a member function does not interfere in which function is chosen when a call is made. If the best fit function is private, it cannot be called unless the call is in another class member function.
```cpp
class A {
	public: void f(int);
	private: void f(char);
};
int main() {
	int n = 0;
	char c = 'b';
	A a;
	a.f(c); // leads to compiler error
}
```
Overload resolution happens before access checking; the compiler matches the a.f(c) call to the function signature `void f(char)` but because it is private, it cannot be accessed, resulting in an error. It does not fall back to the public function.

If access affected overload resolution, then changing a function's status could silently change which function gets called in existing code, which could lead to silent bugs.
### Default Arguments
Member functions can have [[Functions#Default Arguments||default arguments]], which are defined in the member function's declaration, not its definition.
### Inline Member Functions
An inline function is marked with the `inline` keyword. When a function is called, there is overhead: pushing arguments, jumping to the function code, and then returning. For small functions, this overhead can swamp the workload of the function itself. Inlining avoids calling and instead substitutes the body of the function where it was called.

For member functions, we can either provide the function definition within the class statement (avoiding the use of inline) or proceed as normal and provide a definition outside the class statement. In this case, the `inline` qualifier must appear both before the statement and before the header:
```cpp
class Point {
	int x, y;
	public:
		Point(){ x = 0; y = 0; } //Inline constructor
}
```