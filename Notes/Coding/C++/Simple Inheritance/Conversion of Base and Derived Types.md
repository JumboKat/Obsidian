A derived type can always be converted to the base type:
```cpp
class Point{...};
class Pointcol : public Point {...};

Point a;
Pointcol b;

a = b; // legal
b = a; // illegal
```
The assignment is made either using the assignment operator of the base class `Point` (over-defined or default otherwise). However, the inverse `b = a` is rejected.
### Converting Pointers
A similar situation arises when converting pointers to the base and derived types:
```cpp
Point *adp;
Point *adpc;

adp = adpc; // legal
adpc = adp; // rejected
```

The reverse assignment is possible if we use the [[Type Conversions#Cast Operator||cast operator]]:
```cpp
adpc = (Pointcol*) adp; //legal
```

A pointer on the base class can be converted to a pointer on the derived class. Since `Point*` is just an address, and a `Pointcol` object begins with a `Point` part, it can reinterpret the address. However, a base class object cannot be converted into a derived class object. Since the derived object `Pointcol` has more members than `Point`, the compiler does not know what to assign to these extra members. This is only made possible by a user-defined conversion 
(`Pointcol(const Point&)`). 

A derived-to-base conversion is done implicitly, but only for public derivations. Since every `Pointcol` contains a `Point`, we simply just keep the `Point` part and remove the extra members. A public derivation means "a `Pointcol` is a `Point`," so we can substitute one for the other. A private derivation means "implemented using `Point`," and the outside world isn't supposed to know about the relationship. Allowing an implicit conversion to `Point` would allow outside code to reach its members and bypass the restriction.
#### Limitations Involving Static Typing of Objects
A pointer to the base class will only access the base class' members. Take the following example:
```cpp
Point p(3, 5);
Pointcol pc(8, 6, 2);
adp = &p;
adpc = &pc;
```
This can be visualized as:
![[Pasted image 20261126185836.png]]
The instruction `adp->display()` calls `Point::display()` and `adpc->display()` calls `Pointcol::display()`. These are equivalent to `p.display()` and `pc.display()`, respectively.

However, if we do `adp = adpc`, both pointers now point to the same object `pc`.
![[Pasted image 20261126190008.png]]

The instruction `adp->display()` still calls `Point::display()` since adp is of `Point*` type. Any calls on functions unique to the `Pointcol` class (e.g. `adp->colour(8)`) will lead to a compilation error. The type of objects pointed to by `adp` and `adpc` are decided and frozen at compilation time. 