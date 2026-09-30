While we cannot place the name of a function in a variable, we can create a pointer to a function. As with array names, the name of a function is a pointer containing the address of that function.
### Call Setting
The example statement below:
```cpp
int (*adf)(double, int);

int fct(double, int);
adf = fct;
```
Declares a pointer adf that points to a function that takes two parameters (a double and an int) and returns an integer. The pointer can be assigned to any function that fits this description.
### Functions as an Argument
We can write a function that takes a function as one of its parameters like so:
```cpp
float integ(float(*f)(float), ...)
```
Here, the first parameter takes the address of a function that takes a float as a parameter and returns a float.