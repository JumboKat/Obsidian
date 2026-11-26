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

While the derived can always be converted to a base in all cases, the reverse is not always true. A pointer on the base class can be converted to a pointer on the derived class. Since `Point*` is just an address, and a `Pointcol` object begins with a `Point` part, it can reinterpret the address. However, a base class object cannot be converted into a derived class object. S