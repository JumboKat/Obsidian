By default, the `=` operator copies the values of the second operand to the first. However, if the objects involved contain pointers on dynamic locations, this default behavior is not sufficient and requires over-definition. 
### The Shallow Copy Problem
The default [[Copy Constructor||copy constructor]] and default assignment operator perform the same job of copying an object's values. In the case of the default copy constructor, a shallow copy of an object with pointers would simply copy the pointer but not the data pointed to, resulting in both pointers of each object pointing to the same address. A solution for this was to define a copy constructor that copied both the object and its dynamic part in a new location (or, simply prohibit copying).

Assignment between two different objects suffers from a similar issue. For example: `Vect a(5), b(3);` allocates memory in the following way:
![[Pasted image 20261108164637.png|340]]

The assignment `b=a;` leads to the following:
![[Pasted image 20261108164716.png|352]]

This is similar to the shallow copy problem, except now there are two full objects, with one pointing to the dynamic memory space of the other and abandoning its original memory is lost.
### Deep Copy
With the assignment `b = a`, each object should have its own block on the heap with identical contents, each with their pointers pointing to different blocks. To achieve this we must follow the following steps:
1. Release the location currently pointed to by b, since b already exists and owns its own block. We must clean up the old location before reassigning, else it is unreachable and presents a data leak.
2. Allocate a new location for b of a's block size (nelem) and copy a's values into it.
3. Update b's size (nelem) and redirect its pointer to the new block.
```cpp
class Vect {
	int nelem; //number of elements
	int* adr; //pointer to elements
	public:
	Vect(int n) { //constructor
		adr = new int[nelem = n];
		for (int i = 0; i < nelem; i++) adr[i] = 0;
	}
	~Vect() { //destructor
		delete adr;
	}
	Vect& operator=(const Vect&);
};
Vect& Vect::operator=(const Vect& V){
	if (this != &v){ //only if a and b are different
		delete[] adr; //release current block
		nelem = v.nelem; //size = new size
		adr = new int[nelem] //create block of new size
		for (int i = 0; i < nelem; i++)
			adr[i] = v.adr[i]; //copy contents
	}
	return *this; //allows chaining: c = b = a;
}
```

The check `if (this != &v)` prevents an object from being assigned to itself. Without it, 
`a = a` would first free its own memory, then try to copy values from that freed memory and instead read garbage. Self-reassignment is redundant anyway. If such a reassignment was tried, the program does nothing.

While this solution works for this code, it is not a substitute for defining a copy constructor; both are essential in a regular program. The example above passes objects by reference, but if the = operator was instead over-defined as `Vect operator=(Vect v);`, it would make a call to the copy constructor. If one hasn't been defined to handle dynamically allocated memory, a shallow copy would be made.
### Prohibiting Assignment
We can prohibit assignments by:
- Defining it as private (prohibits functions outside of the class from using it)
- Declare the function without providing a definition (Any call is rejected, even by a member function)
It is best to combine the two.
### Canonical Form of a Class
Once a class has pointers on dynamic data, the class must have the ability to perform deep copies (both with the copy constructor and the assignment operator). Thus, the class must have at least the four following member functions:
- Constructor (responsible for dynamic allocation)
- Destructor (responsible for releasing dynamic locations)
- Copy constructor
- Assignment operator

Thus, the **canonical form** of a class is:
```cpp
class T {
	public:
		T(...); //constructor
		T(const T&); //copy constructor
		~T(); //destructor
		T& operator=(const T&); //assignment operator
}
```

It is advisable to use the term `const` for the copy constructor and assignment operator's arguments, since these functions have no reason to change the values of the corresponding objects.

While the assignment operator's argument and return value can be done by either reference or by value, value-by-value transmission requires calling the copy constructor. As long as objects are of respectable size, transmitting by reference is more efficient.