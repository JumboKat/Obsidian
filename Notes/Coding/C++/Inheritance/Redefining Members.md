### Redefining Functions
Since functions in the base class do not account for additional members in the derived class, the derived class can redefine them. For example, `Pointcol` can redefine `display()` to give information about its colour:
```cpp
void display(){
	cout << "I am in " << x << " " << y << "\n"; //error 
	cout << " and my colour is: " << col << "\n";
}
```

However there is a catch; a derived class *cannot* access the private members of the base class. Instead, the display() method can call `Point::display()` to handle its private members:
```cpp
void Pointcol::display(){
	Point::display();
	cout << " and my colour is: " << col << "\n";
}
```
The range operator (`::`) is necessary to call Point's display method; otherwise this leads to a recursive call of Pointcol's display method.

For a member of the derived class, calling its own redefined methods takes precedence over the base class' methods; the latter can be called using the range operator:
```cpp
Pointcol p;
p.display(); //calls Pointcol::display()
p.Point::display();
```
### Redefining Data Members
We can also redefine the base class' data members. If class A is defined as:
```cpp
class A{
	int a;
	char c;
};
```
And class B is defined as:
```cpp
class B : public A {
	float a;
};
```
Then if an object b is of type B, `b.a` refers to the float type of member b, but we can still access the int-type member belonging to class A using `b.A::a`. Redefining a member in B adds to the same member in A instead of replacing it.
### Redefinition and Over-definition
If a function is overloaded in the base class A and redefined in the derived class B, the redefined function in the derived class is always called instead of those in the base class, (if called by an object of B-type) even if the call better suits a signature in A:
```cpp
class A{
	public:
	void f(int n){...}
	void f(char c){...}
};
class B : public A{
	public:
		void f(int n){...}
};

int main(){
	int n; char c; A a; B b;
	b.f(n); //calls B:f(float) even if A:f(int) is a better match
	b.f(c); //calls B:f(float)
}
```

Even if calling the redefined function in B would lead to a compilation error, A's functions are still ignored:
```cpp
class B : public A {
	public:
		void f(int, int){...}
};
int main() {
	int n; char c; B b;
	b.f(n); // compilation error
	b.f(c); // compilation error
}
```

Redefining a function in a derived class hides all member functions of the same name in the base class (and all ancestor classes). The search for a function is only ever done within a single class; this can be the derived, base, or ancestor class, but never several at once (unless we use `using`). This feature can be used to prevent calls to a function of a base class.

We can impose that a search for an over-defined function is done in several classes using `using`. We place this in the base class whose functions we want to call. In this example, we would use `using A::f` within class A, allowing all functions with the name f to be called by any derived class: `b.f(c)` then calls `A::f(char)`.