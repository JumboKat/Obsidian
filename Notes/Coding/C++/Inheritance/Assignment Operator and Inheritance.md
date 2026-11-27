How the = operator is handled depends on whether the derived class over-defines it or not.

If the derived class does not over-define the = operator, a call is automatically made to A's = operator (whether over-defined or default) to handle the inherited A parts, while the members unique to B are handled based on their type (e.g. int uses built-in assignment, strings use String's operator=). If A's = operator was defined private with no body to prohibit assignment in A, then assignment in B will also be prohibited.

If the derived class does over-define the = operator, A's will not be called. Thus, B's = operator must handle all members of B, including members inherited from A.

If we want to use A's = operator to handle the A parts within B's = operator, we can call it explicitly via [[Conversion of Base and Derived Types||pointer conversion]]:
```cpp
Pointcol& operator = (Pointcol& b){
	Point * ad1, *ad2;
	ad1 = this;
	ad2 = &b;
	*ad1 = *ad2; //assigning the Point part of b
	col = b.col; // assigning the Pointcol part of b
	return *this;
}
```

Within the operator= function, we create two A* pointers, then assign one of them to `this` and the other to the B-type reference. We then assign the two pointers to each other, calling A's =operator. Then we explicitly handle B's unique members.