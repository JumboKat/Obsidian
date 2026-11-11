We can over-define the `[]` operator to allow an object to be indexed like an array.
### The Alternative
Without operator overloading, we can define functions to access an object's array by position:
```cpp
void range(int value, int position); //store value at position
int find(int position); //read value at position
```

This works, but is very clunky, as we need a separate function for reading and writing.
```cpp
Vect a(5);
a.range(15, 0);
a.find(0);
```
### Over-Defining
We can over-define the `[]` operator to access an element in a dynamic array by index:
```cpp
int& operator[](int i) { return adr[i]; }
```

This allows us to write:
```cpp
a[0] = 15;
a[1] = 25;
cout << a[0];
```

We must return a reference (int&) to allow its use as an **lvalue**, when we want to write to an index `a[i]`. This way, a single reference-returning operator handles both the read and the write. Returning by reference is safe since the memory is dynamically allocated on the heap and outlives the function call.

It is required that `operator[]` be a member function, since its first operand (`a` in `a[i]`) is the object itself (`this`), with the argument being the index i. Hence, `a[i]` is interpreted as `a.operator[](i)`.

While the second operand (i) can be transmitted by reference, this provides no real benefit given an int variable is of small size, so copying isn't an issue. 

The `[]` operator cannot be defined as a friendly function; the same holds true for the [[Assignment Operator (=)]]. Additionally, only member functions with the `const` designation can be applied to a constant object. Therefore, the `[]` operator does not allow access to a constant object. For this, a second definition should be made for constant objects, allowing us to view it without modifying:
```cpp
int Vect::operator[](int i) const {
	return adr[i];
}
```

An assignment such as `v[i]= ...` with v being a constant would be rejected since the return value of the constant version of the operator is a temporary copy, not the actual element, so there is nothing to store it in.