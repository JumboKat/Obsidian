To use input/output, we must include the header file:
```cpp
#include <iostream>
#include <istream> //if only input is used
#include <ostream> //if only output is used
```
### Printing on the Screen
The syntax is as follows: 
```cpp
std::cout << expr1 << ... << exprn << std::endl;
```
- **cout** refers to standard output.
- **<<** is an binary operator. In this case it is overloaded, and is used to direct values to output.
- **endl** is used to display a line break; it is optional and can be placed anywhere in the output stream.
### Reading from the Keyboard
The syntax is as follows:
```cpp
std::cin >> var1 >> ... varn;
```
- **cin** represents input characters typed from the keyboard, which are stored in a buffer
- **>>** directs input from the buffer to a variable
- Spaces, tabs, and line ends (i.e. enter key) are separators and mark the end of one input for one variable; the cin buffer will then be assigned to the next variable in sequence.
- 