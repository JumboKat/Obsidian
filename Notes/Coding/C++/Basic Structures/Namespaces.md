Due to the number of different libraries, functions, etc., C++ uses **namespaces** to avoid name conflicts. A namespace is a defined set of variables, types and functions.

Anything belonging to a namespace has a full name of the following convention:
```cpp
namespace::name

//example: a function int fct() in the A namespace will have the full name:
A::fct()
```

We can avoid writing the full name with the statement:
```cpp
using namespace name;
```
#### Creating a Namespace 
The following example declares a namespace "toto" along with a variable and a function.
```cpp
namespace toto {
	int N = 10;
	void test(){
		std::cout << "test...";
	}
}
```

The full names of the variable and function are **toto::N** and **toto::test**, respectively. We can simply write their names if we use the statement **using namespace toto;** near the top of the file.