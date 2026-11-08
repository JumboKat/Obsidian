By default, the `=` operator copies the values of the second operand to the first. However, if the objects involved contain pointers on dynamic locations, this default behavior is not sufficient and requires over-definition.
### The Shallow Copy Problem
The default [[Copy Constructor||copy constructor]] and default assignment operator perform the same job of copying an object's values. In the case of the default copy constructor, a shallow copy of an object with pointers would simply copy the pointer but not the data pointed to, resulting in both pointers of each object pointing to the same address. A solution for this was to define a copy constructor that copied both the object and its dynamic part in a new location (or, simply prohibit copying).
### Assigning Two Different Objects
Assignment between two different objects suffers from a similar issue. For example: `Vect a(5), b(3);` allocates memory in the following way:
![[Pasted image 20261108164637.png|340]]

The assignment `b=a;` leads to the following:
![[Pasted image 20261108164716.png|352]]

This is similar to the shallow copy problem, except now there are two full objects, with one pointing to the dynamic memory space of the other and abandoning its original memory is lost.
### 