An **enumeration** is a special case of int type. Like [[Structures||structures]], they are a type defined by the programmer. An enumeration contains a set of strings that each correlate to an integer.
```cpp
enum Colour {yellow, red, blue, green}; //define the enum type Colour
Colour c1, c2; //create two Color-type variables 

c1 = yellow;
c2 = c1;
int c3 = red;
int c4 = 4*red + green;
```
We can manipulate an enum's strings as if they were an integer. However, since they are still constants, we cannot modify their value after creation.

By default, the value of enum strings start at zero and increment by one for each additional element. However, their values can be explicitly set:
```cpp
enum Colour {yellow = 5, red, blue = 6, green, orange = 0, purple};
//yellow=5, red=6, blue=6, green=7, orange=0, purple=1;
```
There are no restrictions against two different strings being assigned the same value.