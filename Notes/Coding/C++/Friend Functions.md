Normally, a member function of one class cannot access the private members of another class. When defining a class, it is possible to declare **friend functions** that belong outside the class that are allowed to access private data, just like any member function.

There are several situations for friend functions:
- Independent function, friend to a class.
- A member of one class, friend to another class.
- A friend of several classes
- All member functions of one class, friends to another class.
### Independent Function
