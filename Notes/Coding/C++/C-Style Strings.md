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
### Reading and Writing Strings
We can use character arrays to store strings:
```cpp
char lastNamep[20], firstName[20];
cout <<"What is your first and last name?: ";
cin >> lastName >> firstName;
```
Some things to note:
- The character array must store n+1 characters in memory, so the input provided by the user must not exceed 19 characters.
- Input by the user is sent to a write buffer, which uses spaces or end of line (Enter) to separate inputs. This means that we cannot fully read a string containing a space or end of line, though there are ways around this using the **getline** method.
### Initializing Strings as Arrays
We can initialize an array of characters in any of the following ways:
```cpp
char ch[20]; //reserves a block of 20 consec. char in memory. This block's location can't be moved, so we can't reassign the ch. We can change its contents with ch[0] = 'h'.
char ch[20] = "hello";
char ch[20] = {'h', 'e', 'l', 'l', 'o', '\0'};
char message[] = "hello"; //size is determined by initialization+1
char *ch = new char[20] //we cannot call ch="hello"; this re-points ch to the string literal "hello", while the array on the heap is now unreachable. This causes an error.
```
### Initializing an Array of String Pointers
To the compiler, a string literal like "hello" is just an address. This means we can put several of them into an array of pointers, with each element of type char \*:
```cpp
const char *day[7] = {"monday", "tuesday", "wednesday", "thursday", "friday", "saturday", "sunday" };
```
This creates 7 constant strings and initializes the day array that holds the addresses of these strings. While we can reassign the pointers, we cannot change the strings themselves. So:
```cpp
day[0] = "Hello"; //valid
day[0][0] = 'c'; //invalid
```
### Passing Arguments to main()
The *main* function can be provided arguments by the program on launch. These arguments are always C-style strings, following this convention:
```cpp
int main(int nbarg, char* argv[]);
```
- The first received argument is an int representing the total number of parameters provided in the command line (the name of the program counts itself as a parameter).
- The second argument is the address of a pointers array, each pointing to the string corresponding to each parameter. For the example call "test arg1 arg2 arg3":
	- at argv\[0], the string test.
	- at argv\[1], the string arg1,
	- etc...
### Functions for C-Style Strings
Since C-style string is not a type, we can never pass the value of a string, but rather its address/pointer to the first character. The following functions have been inherited from C:
- **strcmp**: compare the contents of two strings.
- **strcpy**: copy a string from one location to another.
- **strlen**: return the length of a string (not including zero code).