### Constructor
Only data of static objects are initialized to zero. Thus, a member function must be used to assign values to the object. A **constructor** is a special [[Member Functions||member function]] that is systematically executed as soon as any object is declared.

The constructor has the same name as the class:
```cpp
class Point{
	int x;
	int y;
	public:
		Point(int, int);
		void move(int, int);
		void display();
}

Point:Point(int abs, int ord){
	x = abs; y = abs;
}
```

A class may have any number of different constructors; they will all have the same name, but take different parameters. If a class has declared *any* constructor, it is not possible to create an object without providing the arguments required by the constructor. This means that `Point a;` is no longer possible (unless there is a constructor with no arguments, in which case it is correct, but `Point a()` is not).

A constructor can be overloaded or have default arguments. If a class does not define a constructor, it has a default constructor (do nothing).
### Destructor
The destructor is a member function that is systematically called at the end of an object's life. It takes no parameters and it releases the memory allocated to the object. All classes have a destructor by default if one is not defined. Additionally, the act of releasing allocated memory does not need to be explicitly defined. It shares the same name as the class, preceded by the tilde (~).
```cpp
class Test {
	public:
		int num;
		Test(int);
		~Test();
};

Test::Test(int n){
	num = n;
	cout << "++ constructor call\n";
}
Test::~Test(){
	cout << "-- destructor call\n";
}

```

### Rules
- A constructor is not limited to initializing values with the arguments provided. It can execute any desired code that is defined. For instance, a constructor can dynamically allocate member functions.
- For dynamically allocated members, the assignment between objects using the = operator does not copy dynamic values (a=b). To do this requires overloading the operator=.
- A destructor cannot take any arguments.
- Neither a constructor nor a destructor returns any value. No type should be specified before the name, even **void**.
- Both can be public or private, though it is better to make them public.
	- If a destructor is private, it can no longer be called directly, which is usually not serious as this is rarely useful.
	- Privatizing a constructor means it can only be used by class member functions.
- In Java, members are always initialized to zero and can also be initialized when they are returned, two properties that do not apply in C++.