The [[Copy Constructor||copy constructor]] is used to initialize an object from another existing object of the same type. It is also called when transmitting the value of an object in the argument and/or return of a function (to make for temporary copies). What happens depends on whether the derived class defines its own copy constructor or not.
### Derived Class Does not Define a Copy Constructor
If the derived class B does not define a copy constructor, the default copy constructor is called. This copy constructor copies the members that belong solely to B, and invoke the base copy constructor to handle the parts derived from A (by [[Constructor and Destructor Calls#Transmitting Information Between Constructors||transmitting information between constructors]]). If A did not define a copy constructor, then its default is called for this purpose. If A declares a private copy constructor without definition (to prevent copying of A objects), then B will also be prevented from being copied.
### Derived Class Defines a Copy Constructor
The copy constructor of the derived class, if defined, must handle the copying of the entire object, not just the inherited part. We can still use the technique of [[Constructor and Destructor Calls#Transmitting Information Between Constructors||transmitting information between constructors]] to handle the inherited parts. So, the B copy constructor can provide information to A's constructor:
```cpp
B (B &x) : A(...){...}
```
Ideally, we call A's copy constructor rather than its default. This would require passing a reference to not the entire object, but only the parts of it that are type A. This is where [[Conversion of Base and Derived Types||implicit conversion]] of a derived class into its base class comes into play. It allows us to define the copy constructor as such:
```cpp
B (B &x) : A(x){ //when passed to A, x is implicitly converted into an A-type, keeping only the base members.
// B's copy constructor handles the copying of the B-specific parts
...}
```
