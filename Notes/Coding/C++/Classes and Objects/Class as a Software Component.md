Typically we declare a class outside of the program it is used in in a separate .h header file. We can use it with \#include, and define its members (especially static members) in the main program. 
```cpp
//surround with <> for standard header files
#include <iostream> 
//surround with "" for user-defined header files
#include "Point.h" 
using namespace std;

//define point class member functions
Point::Point(int abs, int ord) {}
...
```
### Multiple Inclusions
A header file can be processed more than once in the same compilation, if it is included in multiple header files that are also then included in the same file:
```cpp
// circle.h
#include "point.h"

// line.h
#include "point.h"

// main.cpp
#include "circle.h"
#include "line.h" //point.h is included a second time

```

Without protection, main.cpp sees the Point class declared twice, and the compiler gives an error for redefining the Point class.

We can use a guard to prevent this:
```cpp
#ifndef POINT_H //if point_h is not defined yet...
#define POINT_H //define it now and keep going.

// declaration of point class

#endif //end of conditional block
```

This means that on the second inclusion, it is already defined, so \#ifndef evaluates to false, and the class declaration is skipped.