A pointer in C++ is a variable that holds an address (of a variable, function, etc). A pointer is declared like so:
```cpp
int * address
int *address //we can have a space or not
```
We declare a type, followed by "\*", then the variable name. The type defines the type of the value stored at the address the pointer contains.
### Address and Reference Operators
Since pointers are variables, we can manipulate them like we would with any variable:
```cpp
int *ad
int n;
n = 20;
ad = &n; // or & n
*ad = 30; //or * ad = 30; these are high priority operators AND separators, so we do not need a space nor brackets.
```
We can declare them (with or without initialization) and assign them values. We use the address-of operator (**&**) to return the address of a variable. We use the dereference operator (**\***) on a pointer to return the value stored at the pointer's stored address.

We can declare multiple pointers in the same statement like any other variable:
```cpp
int *ad1, *ad2, *ad;
int * ad1, ad2, ad; //these are distinct; this only creates one pointer and two int variables.
```
### Operations
We can also perform operations with them:
```cpp
int n=10, p=20;
ad1 = &n;
ad2 = &p;
*ad1 = *ad2 + 2; //this means set the value of n to p + 2;
*ad1 += 3; //this is the same as n = n + 3;
(*ad1)++; //this is the same as n++;
```
#### Incrementing Pointers
We can increment pointers themselves:
```cpp
int *ad;
ad + 1; // or ad++;
```
This reassigns the pointer to the address of the next Integer. This is useful for processing arrays and Strings.

We can increment or decrement a pointer by any integer quantity:
```cpp
ad += 10;
ad -=25;
```
### Passing by Address
In addition to passing by value and by reference, we can pass arguments by their address (pointers):
```cpp
//Permutation Example
#include <iostream>
using namespace std;
int main() {
	void exchange(int *ad1, int *ad2);
	int a=10, b=20;
	cout << "Before call:" << a << " " << b << endl;
	exchange(&a, &b);
	cout << "After call: " << a << " " << b << endl;
}

void exchange(int *ad1, int *ad2){
	int x;
	x = *ad1;
	*ad1 = *ad2;
	*ad2 = x;
}
```
In this example, we pass the addresses of two variables as parameters. The function then uses the dereference operator to swap the values of the two variables. Like passing by reference, we are able to modify the values of the original variables this way.