### Constructor
Only data of static objects are initialized to zero. Thus, a member function must be used to assign values to the object. A **constructor** is a special member function that is systematically executed as soon as any object is declared.

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

A constructor is not limited to initializing values with the arguments provided. It can execute any desired code that is defined.
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

