### Overloading
All member functions, including [[Constructor and Destructor||constructors]] can be [[Functions#Overloaded Functions||overloaded]]. The status (private/public) of a member function does not interfere in which function is chosen when a call is made. If the best fit function is private, it cannot be called unless the call is in another class member function.
```cpp
class A {
	public: void f(int);
	private: void f(char);
};
int main() {
	int n = 0;
	char c = 'b';
	A a;
	a.f(c); // leads to compiler error
}
```
Overload resolution happens before access checking; the compiler matches the a.f(c) call to the function signature `void f(char)` but because it is private, it cannot be accessed, resulting in an error. It does not fall back to the public function.

If access affected overload resolution, then changing a function's status could silently change which function gets called in existing code, which could lead to silent bugs.
### Default Arguments
Member functions can have [[Functions#Default Arguments||default arguments]], which are defined in the member function's declaration, not its definition.
### Inline Member Functions
An inline function is marked with the `inline` keyword. When a function is called, there is overhead: pushing arguments, jumping to the function code, and then returning. For small functions, this overhead can swamp the workload of the function itself. Inlining avoids calling and instead substitutes the body of the function where it was called.

For member functions, we can either provide the function definition within the class statement (avoiding the use of inline):
```cpp
class Point {
	int x, y;
	public:
		Point(){ x = 0; y = 0; } //Inline constructors
		Point(int abs) { x = y = abs; }
		Point(int abs, int ord) { x = abs; y = ord;}
		void display(const char* = "");
}
```
 
 or proceed as normal and provide a definition outside the class statement. In this case, the `inline` qualifier must appear both before the statement and before the header:
 ```cpp
 class Point{
	 public:
		 inline Point();
		//...
 };
 inline Point::Point() {x = 0; y = 0;}
 ```

We can also declare an inline constructor with default arguments.
### Objects as Arguments
Like any other variable, we can pass objects as arguments, including by address and by reference. Transferring by value passes a simple copy.
### Returning an Object
Member functions can return an object, either by value, address, or reference. If it is of the same type as the class, the function will have access to its private members. Otherwise, it may only access its public members.
- Passing by value simply makes a copy of the object. For objects with pointers on dynamic locations, a particular constructor is required (the pointed value is not copied)
- Returning an object that was locally created (automatic) by address or by reference will simply return the address of something that doesn't exist, as the object is deleted once the function call ends. 
```cpp
Point point::symmetric(){
	Point res;
	res.x = -x; res.y = -y
	return res;
}
```
This example creates an automatic class and returns a copy of it. It would not be advisable to return a reference to it with `Point& Point::symmetric()`.
### Self-Referencing: keyword this
Every member function has an implicit argument called `this` which is a pointer to the object that called the function; it returns the address. This is unlike Java, where `this` refers to the object itself.
### Static Member Functions
Member functions that are declared static in the class statement can only access other static members; even `this` is excluded.
```cpp
class Integer{
	int i;
	static int j;
	public:
	static int get_value(void);
};
int Integer::j = 0;
int Integer::get_value(void){
	j = 1;
	return i; //Error: get_value cannot access i.
}
```

Static members can only access members that belong to the *class*, not an object of the class. Because static member functions do not work on class objects but the classes themselves, we can call it without referencing an object; we can even call it if no objects exist. We simply use the range resolution `::` operator like so: `class::function()`
### Constant Member Functions
We can define objects as constant objects. The class statement must specify which functions are allowed to operate on constant objects like so:
```cpp
class Point{
	int x, y;
	public:
	Point(...);
	void display() const;
	void move(...);
	//...
};
```

Declaring a member function is constant means that it can be used by constant objects (other functions cannot):
```cpp
Point a;
const Point c;
a.display();
c.display();
a.move(...);
c.move(...); //this raises a compiler error; c is constant but move is not.
//the same holds true for const objects as arguments
void f(const Point){
	p.display();
	p.move(...); //error
}
```

A constant member function cannot modify the value of data members (except static members, since they are not associated with any particular object):
```cpp
class Point {
	int x, y;
	static int n;
	public:
	void display() const{
		x++; //error
		n++; //OK since n is static
	}
};
```

Member functions can be overloaded with a const and non-const version. Non-const objects will call the non-const function, while const objects will call the const function. If no const version exists, const objects cannot call it. However, both const and non-const objects can call a const function.