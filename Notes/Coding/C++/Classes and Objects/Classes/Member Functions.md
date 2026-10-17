### Overloading
All member functions, including [[Constructor and Destructor||constructors]] can be overloaded. The status (private/public) of a member function does not interfere in which function is chosen when a call is made. If the best fit function is private, it cannot be called unless the call is in another class member function.
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
Member functions can have default arguments, which are defined in the member function's declaration, not its definition.