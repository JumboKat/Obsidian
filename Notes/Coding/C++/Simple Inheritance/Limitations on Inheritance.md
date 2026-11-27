When a derived class publicly inherits a function, it is able to use that function freely, but the function behaves as it is defined in the base class; it has no knowledge about the derived class.
### Return Type
```cpp
class A { 
	public: 
		A f(); //return type A
};
class B : public A {};

B b;
A r = b.f();
B r2 = b.f();
```
In this case, the return type of f is an object of A-type. This can change the way the function can be used. Extra data that exists in only B is not present.
### Accepted Arguments
Take the function in A defined as `t f(A)` and the variables `B b1, b2; A a1;`. If we call `b1.f(a1)`, there are no issues (exact type). However, calling `b1.f(b2)` requires an implicit derived-to-base conversion. 
- If f takes its arguments by value, b2 is copied into an A (using the A copy constructor).
- If f takes it by reference, a B& converts to an A&.
This only works if the inheritance is public.
### Operator+
`Point` defines `operator+` and `Pointcol` inherits it publicly:
- If it is a member function of Point: `a + b` calls `a.operator(b)`.
- If it is a friend function: `a + b` calls `operator+(a, b)`. 

In either case, the result is a Point, even if both operands were Pointcol.
```cpp
Pointcol a, b, c;
c = a + b; //error result is a Point, there is no Point->Poincol conversion
```

The fix is to redefine `operator+` in `Pointcol`.

