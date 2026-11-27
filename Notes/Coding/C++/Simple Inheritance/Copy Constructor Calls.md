The [[Copy Constructor||copy constructor]] is used to initialize an object from another existing object of the same type. It is also called when transmitting the value of an object in the argument and/or return of a function (to make for temporary copies). What happens depends on whether the derived class defines its own copy constructor or not.
### No Defined Copy Constructor
If the derived class B does not define a copy constructor, the default copy constructor is called. This copy constructor copies the members that belong solely to B, and invoke the base copy constructor to handle the parts derived from A. If A did not define a copy constructor, then its default is called for this purpose. If A declares a private copy constructor without definition (to prevent copying of A objects), then B will also be prevented from being copied.
### 

