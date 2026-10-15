A class is a [[Structures||structure]] where members and member functions are by default private. 
### Declaration
Its declaration is similar to that of a structure's, except we replace the word **struct** with **class**. Members are grouped based on whether they are public or private; since private is the default security level, it does not need to be explicitly declared.

After the class's declaration, we define its member functions below and outside of the declaration scope. We use the syntax ``type class::member_function`` when defining to a member function.

Example:
```cpp
class Point{
/* statement of private members*/
	private: //optional
		int x;
		int y;
// statement of public members
	public:
		void initialize(int, int);
		void move(int, int);
		void display();
};

//After declaration, we define class members (functions)
void Point::initialize(int abs, int ord){
	x = abs; y = ord;
}

void Point::move(int dx, int dy){
	x = x + dx; y = y + dy;
}

void Point::display(){
	cout << "Coords: " << x << y << "\n";
}

int main(){
	Point a, b;
}
```

A variable declared as a class type is called an **instance** or **object** of that class. In the example above, because the Point class fields x and y are declared private, we cannot call "a.x" outside of the class's scope as we cannot access the value x, leading to an error.
### Assigning Objects
Like structures, we can initialize a new class instance with an existing one. The assignment:
```cpp
Point a, b;
b = a;
```

Copies the members of a into b. In this case, the values of x and y. Because these are private fields, this cannot be replaced by:
```cpp
b.x = a.x;
b.y = a.y;
```

Regardless of status, the assignment a = b is always legal.