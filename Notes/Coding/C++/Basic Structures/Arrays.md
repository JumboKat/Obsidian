An array is a set of items of the same type designated by a single identifier; each is identified by an index specifying its position in the set. Arrays are of fixed size that must be constant at compile-time. They occupy a contiguous block in memory. The array decays to a pointer to its first element in most expressions (e.g. passing it to a function).
### Defining 1-Dimensional Arrays
To declare an array, the following syntax is used: type name\[size]. The following are all valid declarations:
```cpp
//array of 5 int values
int tab[5] = { 10, 20, 5, 0, 3 }; 

//if size is not explicit, it is defined by initialization
int tab[] = { 10, 20, 5, 0, 3 }; //size is 5 (5 variables)

//if array not fully initialized, value will default (0, false, etc)
int tab[5] = { 10, 20, 5 }//equivalent to tab[]={10,20,5,0,0}
```
There are no bounds checking; accessing outside of an array's range is undefined and will lead to an execution error.
### Defining Multi-Dimensional Arrays
```cpp
//An array of size three, with each element being an int array of size 4
int tab[3][4] = {
	{1,2,3,4}.
	{5,6,7,8},
	{9,10,11,12}
};
//Nested braces are optional, so this is equivalent
int tab[3][4] = { 1,2,3,4,5,6,7,8,9,10,11,12 };

//At each level, the last values can be omitted
int tab[3][4] = {{1,2}, {3,4,5}}; // = {{1,2,0},{3,4,5},{0,0,0}}

//When nested braces are omitted, elements are grouped to fill left-most arrays first, so the following is valid but not equivalent:
int tab[3][4] = { 1, 2, 3, 4, 5 };//={{1,2,3},{4,5,6},{0,0,0}}
```
### Static vs Automatic Arrays
When initializing static arrays, initial values must be constant expressions (evaluated at compile time), which includes constant literals and variables, as well as other static variables:
```cpp
void f() {
	const int N = 10;
	static int delta = 3;
	.....
	int tab[5] = {2*N-1, N-1, N, N+1, 2*N+1}; //={19,9,10,11,21}
	int t[3] = {0, delta, 2*delta};
}
```
This is allowed because N is a const and delta is a static; both values are known by the compiler, and expressions involving them can be computed at compile-time.

For an automatic array (array defined within a function), values can be determined at runtime.
```cpp
const int NEL = 10;
void fct(int p){
	int n = 1;
	.....
	int tab[] = { NEL, p, 2*p, n+1, n+p }
}
```
Here, the value of p is not known until fct() is called.
### Vectors
Instead of arrays, we can use the vector class by including its header file:
```cpp
#include <vector>
```
To declare an array, we use its constructor, which follows the syntax:
vector\<type> name(size, init)
```cpp
vector<int> T(100, 5); //a vector with 100 elements set to 5
// second parameter is optional

//We can copy the values of another vector in our initialization
vector<type> name1 = name2;
```

The vector class has predefined methods which standard arrays do not have:
- T.size() returns the size of vector T.
- T\[i] returns the i-th element in T.
#### 2-D Vectors
```cpp
//defines a 2-d vector
vector<vector<int>> T; 

//Initializes a 2-d vector
vector<vector<int>> T2(100, vector<int>(50,1));
//A vector of 100 elements, each being a vector of size 50 filled with 1
```