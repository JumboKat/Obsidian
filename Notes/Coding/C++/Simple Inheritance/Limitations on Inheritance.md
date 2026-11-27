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