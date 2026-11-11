The **++ operator** (and --) can be over-defined. If it is, both the prefix and postfix operator must be explicitly and individually defined. If T designates a class type and ++ is defined as a member function:
- The header `T operator++()` is used to define the prefix operator.
- The header `T operator++(int)` is used to define the postfix operator. The int-type argument is a dummy that carries no value and is never used; it is simply to distinguish between the two function headers.
If ++ is defined as a friend function:
- The header `T operator++(T)` is used to define the prefix operator.
- The header `T operator++(T, int)` is used to define the postfix operator.

Example:
```cpp
class Point{
	int x, y;
	public:
	Point(int abs=0, int ord=0) {x=abs; y=ord;}
	Point operator++(){ //prefix
		x++; y++; return *this; //increment, then return current value
	}
	Point operator++(int n){//Postfix; n=0
		Point p = *this;
		x++; y++;
		return p; //increment, return old value
	}
}
```

It is not possible to define only one ++ operator; bost prefix and postfix operators must be defined.