### Pointer Conversions
There are no implicit conversions between pointers. Only explicit conversions (casting) is allowed, though this comes with some risks; some machines impose alignment constraints on the addresses of objects. It can be done in one of two ways:
```cpp
int *pi;
float *pf;
pi = (int *) pf;

//OR

pi = static_cast<int *>(pf);
```
### Generic Pointers (void \*)
A **generic pointer** has no type, and a pointer of any type can be implicitly converted to a generic pointer, like so:
```cpp
void *ad; //generic pointer
int *adi;
void f(void *); //function accepts any pointer type

ad = adi;
f(adi); 
```

The reverse cannot be done implicitly, and requires typecasting.
```cpp
float *adf;
void *ad;
void g(float *);

adf = ad; //illegal
adf = (float *)ad; //valid
adf = static_cast<float *>(ad); //valid
g(ad); //illegal
g((float *)ad); //valid
g(static_cast<float *>(ad)); valid
```

### Pointer Arithmetic
A variable of type void * cannot perform arithmetic operations; if p and q are type void \*, the statements p+i (i being an int), p - q, and p++ are not allowed, as we do not know the size of the pointed objects. We also cannot use the dereference operator * with void \*.

Pointer arithmetic requires the compiler to know the size of the pointed-to type, as this determines the number of bytes to step forward, or how many bytes to read/write on a dereference. Since void has no size, these are not possible. 

The fix is to cast pointers of type void * to that of type char \*. Since char is always guaranteed to be 1 byte in size, the compiler is able to perform arithmetic operations.

Char * specifically is conversion-safe because it will never cause an alignment problem. Some types (like int, double) require their addresses to be aligned to specific byte boundaries (e.g. 4-byte int needs to start at an address divisible by 4) for hardware to access them correctly. Char has the loosest alignment requirement since its size is only one byte, meaning it can be held at any address.