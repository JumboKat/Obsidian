During the execution of a program, data is stored in different sections in memory with different lifespans depending on their scope. For any program, there are three separate regions in memory that data can be allocated to:
- The **Stack**  is where local variables, functions, and return addresses are stored. Whenever the a function or variable is not used anymore, it is deleted. This segment grows and shrinks quickly and automatically as functions are called and returned.
- **Global** variables and functions have their own place in memory. They are allocated memory at compile time; size and location are fixed before the program runs. These exist as long as the program runs. Anything that is declared as **static** also live here.
- The **Heap** is where *dynamically allocated* data is stored. This memory is allocated at runtime using the keyword **new** and freed using **delete**. These calls must be made explicitly. Data here persists until they are explicitly freed using delete.
### new Operator
The keyword **new** is used to indefinitely allocate memory for a given variable or function at runtime.
```cpp
//Example: creating a space for int on the heap, and declaring a pointer that points to this address space. The pointer itself is not in the heap.
int *ad;
ad = new int;

// OR

int *ad = new int;

//Allocating space for a 100-character array, and creating a pointer to point to it.
char *adc;
adc = new char[100];
```
#### Syntax
Using new always allocates space for a specified type in memory and returns a pointer to said space. It is used as such:
```cpp
new type; //allocate space for type and create a pointer of type type* for that space.

new type[n]; //where n is non-negative. Allocates space for n items of specified type and returns a pointer to the first element of this array.

new type* [n]; //creates space for an array of n pointers (or a multi-dimensional array)
```

In the case a new statement fails, a type exception **bad_alloc** occurs.
### delete Operator
The only way to free up space allocated by new is using the **delete** operator. To use delete, we call:
```cpp
delete address //for single objects
delete [] address //for arrays of objects
```

When calling delete on a pointer pointing to the desired address space to delete, we simply delete the data held at the address, not the pointer itself. Thus:
```cpp
delete ad; //this deletes the allocated space for int pointed to by ad, but not ad itself.

delete adc; //deletes the space allocated for the char array.
```

Calling delete on a location already released, or on an address that was not allocated with new will lead to an error.