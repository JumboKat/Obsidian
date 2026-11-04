A copy constructor is used and called to instantiate an object from another existing object (copy its fields into the new object's fields). Every class has an implicit, default copy constructor. However, simply copying an object's fields does not always suffice, and a copy constructor has to be defined explicitly by the programmer.

This is the case when an object contains data that has been dynamically allocated. A simple copy of the object's fields would only copy the pointer to the data, not the data itself. Thus, changing the data for one object would change the data for the other.
### Banning Construction by Copying
If a copy constructor is made private, it can only be accessed by class members. If we want to ban construction by copying, we can declare the constructor as private without providing a definition. This means that it can only be called by class members, and when it is called, its defined behavior is to do nothing, so the copy will fail.
### Syntax
A copy constructor is defined in a similar way to standard constructors. The only difference is that it takes a reference to the object to be copied, meaning the header will be of the two forms:
```cpp
Point(Point &);
//or
Point(const Point &);

//This is illegal
Point(Point);
```

A copy constructor cannot accept an object by value; this passes a copy of the object, which calls the copy constructor (to create the copy), which results in an infinite recursion.

Both of the first two forms above can exist within the same class, however it is better to define the second form:
- A non-const reference can't bind to a constant object, since this would allow the constructor to modify something unmodifiable. Form 1 only works with non-const objects.
- Since a copy constructor has no interest in modifying the copied object, form 2 works for both const and non-const objects since we aren't modifying them in any case.

In Java, objects are handled by reference only, and so the concept of a copy constructor does not exist.
### Object as a Return Value of a Function
When an object is the return value of a function, by default it is a copy using the object's copy constructor. If the object has dynamic fields, this leads to a 'superficial copy', and changes to the copy can be effected on the original.