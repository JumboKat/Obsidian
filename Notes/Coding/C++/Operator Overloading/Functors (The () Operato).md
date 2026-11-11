Objects of a class that overload the `()` operator are called **function objects**, or **functors**, meaning they can be called with function-call syntax.
```cpp
class cl_fct {
	public:
		cl_fct(float x) { ... }
		int operator()(int n, int p){ ... }
};

cl_fct obj_fct1(2.5); // construction, calls the constructor with 2.5
obj_fct1(3, 5); //calls obj_fct1.operator()(3,5)
```

The first line creates the object. The second line looks like a function call, but it is actually calling the `()` operator.
### Rules
- It must be a member function.
- It can take any number of parameters of any types, and return any type. This is unlike most operators, which have a fixed arity.
- It can be overloaded multiple times with different parameter lists.