Static data members are data that are shared among all objects of a class type.
```cpp
class Exple{
	static int n;
	float x;
};

Exple a, b;
```
In this example, both a and b have access to the same n. Since only one copy of a static data member exists among all objects, it *cannot* be initialized by a constructor. It must be **explicitly initialized**; that is, outside the class statement:
```cpp
int Exple::n = 5;
```
This can be done for both private and public static members. Unlike ordinary variables, static members are not initialized to zero by default.
### Static Constants
Static constant members can be in initialized at the time of declaration, but they still must be declared outside the class (with no value) for the reservation of memory.
```cpp
class Exple1 {
	static constant int n = 5;
}
const int Exple1::n; //declaration with no value
```

