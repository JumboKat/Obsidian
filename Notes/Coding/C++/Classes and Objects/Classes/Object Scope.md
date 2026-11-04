**Automatic** objects are created by a statement; they are destroyed at the end of their block/function. **Static** objects are created by a statement located either outside the main function (out of a function) or inside a function but declared `static`. It is destroyed after the program's execution.

### Dynamic Objects
#### Without a Constructor
We can dynamically create an object location in memory and assign its address to a pointer:
```cpp
Point* adr;

adr = new Point;
```

Accessing the members of the object are done like so:
```cpp
adr->initialize(1,3);
adr->display();
adr->x;

//or without -> operator
(*adr).initialize(1,3);
(*adr).display();

//remove object
delete adr;
```
#### With a Constructor
