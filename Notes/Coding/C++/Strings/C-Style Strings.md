In C++, there are two types of strings:
- The class string; a real string type. Has defined functions but is limited in what it can be used for.
- **C-style strings**: inherited from C, it is not its own type, but an array of constant characters.
C-style strings are more versatile and are widely used; they are used to pass arguments to the main function.
### Constant Strings
A string of characters is represented by a series of bytes corresponding to its characters, ending with an additional byte of zero code. The number of bytes in a string is the number of characters + 1.

For the string "Hello," the compiler creates an array of bytes in memory for each char (+ zero string). The expression "Hello" does not represent the string's contents, but rather the address to the first character. We can declare a pointer to store this in:
```cpp
const char *adr; //this placement means the characters are constant, not the pointer
adr = "Hello";
```
In this way, strings act in the same way as an array. A string is simply an array of characters + zero code (adr  ──►  \[ 'H' | 'e' | 'l' | 'l' | 'o' | '\0' ]). Additionally, assigning a string to a pointer only stores its address; thus, we cannot compare strings with == as this only compares their addresses; nor can we copy their value with =.

String literals must be of type const char in c++; they live in read-only memory. Modifying a string literal is undefined behavior and would cause an error.